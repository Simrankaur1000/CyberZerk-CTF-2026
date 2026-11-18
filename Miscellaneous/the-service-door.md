# The Service Door

**Category:** Miscellaneous  
**Difficulty:** Hard  
**Points:** 175  
**Author:** Perceval

### Files Used
- `reponse_http.txt`
- `README_2.txt`

### Solution

**Step 1: Inspect the HTTP response**

Opened `reponse_http.txt` and examined the HTTP response.

The response body contained:

```text
Intranet Tricatel — maintenance in progress.
```

The response body did not contain any useful information.

**Step 2: Inspect HTTP response headers**

While inspecting the response headers, I discovered a non-standard custom header named `X-Service-Door`.

```http
X-Service-Door: Q3liZXJaZXJre3AwcnQzX2QzX3MzcnYxYzNfY2FjaDMzX2RhbnNfdW5fM250M3QzfQ==
```

Its value appeared to be Base64-encoded due to its character format and trailing `==` padding.

**Step 3: Decode the Base64 value**

Used the following Linux command to decode the header value:

```bash
echo 'Q3liZXJaZXJre3AwcnQzX2QzX3MzcnYxYzNfY2FjaDMzX2RhbnNfdW5fM250M3QzfQ==' | base64 -d
```

The decoded output revealed the hidden flag.

### Flag

```text
CyberZerk{p0rt3_d3_s3rv1c3_cach33_dans_un_3nt3t3}
```

The flag uses leetspeak and translates to **"hidden service door in a header."** The other headers, such as `Server`, `X-Powered-By`, and `Set-Cookie`, are standard HTTP response information and do not contain the flag.
