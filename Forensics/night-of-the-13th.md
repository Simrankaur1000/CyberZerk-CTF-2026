# Night of the 13th

**Challenge title:** Nuit du 13  

**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 75  
**Author:** Perceval

## Files

- `access.log` — the server access log provided in the challenge.

## Solution

### 1. Open the log file

Open `access.log` with Notepad.

The file contains many requests made to the server. Most of them are normal requests for pages such as `/login`, `/intranet`, and `/css/app.css`.

### 2. Find the unusual request

Press **Ctrl + F** and search for:

`rapport_confidentiel_rh.zip`

This takes you to the suspicious request on line 94:

```http
45.83.12.207 - - [13/Nov/2026:02:47:03 +0100] "GET /export.php?file=rapport_confidentiel_rh.zip&token=CyberZerk%7BL0g_4n4lys3_Exf1l_C0nf1rm33%7D HTTP/1.1" 200 184320 "-" "curl/8.2.1"
```

### 3. Understand why this request is suspicious

Look at these details:

- **IP address:** `45.83.12.207` — unlike the other requests from internal `10.0.0.x` addresses, this is an external IP address.
- **File:** `rapport_confidentiel_rh.zip` — the requested file is a confidential HR archive.
- **Status code:** `200` — the server successfully responded to the request.
- **Time:** `02:47:03` on November 13.
- **User-Agent:** `curl/8.2.1` — the request was made using curl.

Together, these details identify the successful file export request.

### 4. Read the flag

The request contains a token:

```text
CyberZerk%7BL0g_4n4lys3_Exf1l_C0nf1rm33%7D
```

The values `%7B` and `%7D` represent the opening and closing curly braces in a URL. Replace them with `{` and `}` to get the flag.

## Flag

```text
CyberZerk{L0g_4n4lys3_Exf1l_C0nf1rm33}
```
