# The Signal in the Noise

**Challenge title:** Le Signal dans le Bruit\
**Category:** Post-Quantum\
**Difficulty:** Extreme\
**Points:** 500\
**Author:** Perceval

## File

- `signal.json`

## Challenge Analysis

The supplied JSON contains a Learning With Errors (LWE) instance over the modulus:

```text
q = 3329
A: 80 × 40 matrix
b: 80-element vector
```

It also contains an AES-256-GCM encrypted message, including the nonce and authentication tag.

The system follows:

```text
b = A·s + e (mod q)
```

where `s` is the hidden secret vector and `e` is the small error vector.

## Solution

### 1. Recover the LWE secret

Use a primal lattice attack with Kannan's embedding. Apply lattice reduction with LLL followed by BKZ using `fpylll`.

The reduced lattice reveals a short vector corresponding to the hidden secret. The recovered secret is ternary: every coefficient belongs to:

```text
{-1, 0, 1}
```

Validate the candidate by computing:

```text
e = b - A·s (mod q)
```

The recovered vector satisfies the LWE relation, with error coefficients bounded by 32 in absolute value.

### 2. Derive the AES key

Serialize the signed secret coefficients as decimal integers separated by commas, without spaces.

Example format:

```text
-1,0,1,-1,...
```

Calculate SHA-256 over the UTF-8 bytes of that exact string. The 32-byte digest is the AES-256-GCM key.

### 3. Decrypt the message

Use the derived key together with the nonce, ciphertext, and authentication tag supplied in `signal.json`.

Successful AES-GCM authentication and decryption reveal Marc's message.

## Recovered Message

The message says that Marc eventually spoke to a colleague and then a doctor. Talking did not solve everything immediately, but it meant he no longer had to carry the situation alone and that he is doing better.

It encourages anyone who relates to the message to talk to someone and reminds them that asking for help is not a weakness. It also mentions France's 3114 suicide-prevention helpline, which is free, confidential, and available 24/7.

## Flag

```text
CyberZerk{Par1er_Sauv3_D3s_V13s}
```

## Security Lesson

LWE-based cryptography is designed to resist known classical and quantum attacks when secure parameters and constructions are used. This challenge uses a deliberately recoverable secret and small parameters, allowing lattice reduction to solve the instance. It demonstrates how a weak parameter choice can undermine a post-quantum construction.

The challenge also illustrates that key derivation must be reproducible: the exact serialization of the recovered secret is part of the cryptographic process.
