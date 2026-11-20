# Extension

**Challenge title:** Rallonge  
**Category:** Cryptography  
**Difficulty:** Hard  
**Points:** 200  
**Author:** Perceval

## File

- `token.txt`
- `sha256ext.py`
- `cipher.bin`

## Solution

### Step 1: Identify the vulnerable MAC construction

The challenge uses the following authentication construction:

```python
tag = SHA256(secret || message)
```

The secret is 16 bytes long and is unknown.

This construction is vulnerable to a SHA-256 length-extension attack because SHA-256 uses the Merkle–Damgård construction.

### Step 2: Inspect the provided information

The file `token.txt` contains:

- The original message.
- Its SHA-256 authentication tag.
- The secret length: 16 bytes.
- The suffix to append: `&role=admin`.

The objective is to create a valid authentication tag for a modified message without knowing the secret.

### Step 3: Perform the length-extension attack

Use the provided `sha256ext.py` helper.

The attack works by:

1. Loading the original SHA-256 digest as the internal hash state.
2. Calculating the correct glue padding for the original secret and message.
3. Continuing the SHA-256 computation with `&role=admin`.
4. Producing a new valid authentication tag.

The resulting forged message contains the original message, the glue padding, and the appended administrator role.

### Step 4: Obtain the forged tag

The generated authentication tag is:

```text
f958fa939ff8c0437e5c00447963ba4978f0474e62f065517b92f97d2ec90cd7
```

This 32-byte digest is then used directly as the AES-256-ECB decryption key.

### Step 5: Decrypt the encrypted vault

Decrypt `cipher.bin` using the forged digest as the AES-256-ECB key.

The decrypted data has valid PKCS#7 padding, confirming that the recovered key is correct.

The plaintext reveals the flag.

## Flag

`CyberZerk{l3ngth_3xt3ns10n_f0rg3_l4_s1gn4tur3_du_c00k13}`

## Security Lesson

Do not construct message authentication codes using `SHA256(secret || message)`. Use HMAC-SHA256, which is designed to prevent this length-extension vulnerability.
