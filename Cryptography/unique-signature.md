# Unique Signature

**Challenge title:** Signature Unique  
**Category:** Cryptography  
**Difficulty:** Extreme  
**Points:** 275  
**Author:** Perceval

## File

- `signatures.json`
- `vault.bin`

## Solution

### Step 1: Inspect the ECDSA signatures

The challenge provides two ECDSA signatures in `signatures.json`.

Both signatures contain the same `r` value.

This is a strong indication that the signer reused the same nonce `k` when generating both signatures.

ECDSA requires a unique, unpredictable nonce for every signature. Reusing a nonce can expose the signer's private key.

### Step 2: Calculate the message hashes

For each signed message, calculate its SHA-256 hash.

Reduce each hash modulo the secp256k1 curve order `n`:

```text
z1 = SHA256(message1) mod n
z2 = SHA256(message2) mod n
```

### Step 3: Recover the reused nonce

Using the two signatures, calculate the shared nonce:

```text
k = (z1 - z2) × inverse(s1 - s2) mod n
```

Here, `inverse()` represents the modular multiplicative inverse.

### Step 4: Recover the private key

Once the nonce is recovered, calculate Didier's private key:

```text
d = (s1 × k - z1) × inverse(r) mod n
```

This exposes the private signing key because the same nonce was reused.

### Step 5: Derive the AES key

Convert the recovered private key to its decimal string representation and calculate its SHA-256 hash:

```text
key = SHA256(str(d))
```

The resulting 32-byte digest is used as the AES-256-ECB decryption key.

### Step 6: Decrypt the vault

Decrypt `vault.bin` using the derived AES key.

Valid PKCS#7 padding confirms that the recovered key is correct.

The decrypted plaintext reveals the flag.

## Flag

`CyberZerk{ECDSA_m3m3_n0nc3_d0nn3_la_cl3_pr1v33}`

## Security Lesson

Never reuse an ECDSA nonce. Use a secure nonce-generation method, such as deterministic nonce generation following RFC 6979, to prevent private-key recovery through nonce reuse.
