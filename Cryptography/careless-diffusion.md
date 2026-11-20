# Careless Diffusion

**Challenge title:** Diffusion Imprudente  
**Category:** Cryptography  
**Difficulty:** Medium  
**Points:** 125  
**Author:** Perceval

## File

- Challenge-provided RSA ciphertexts and public moduli

## Solution

### Step 1: Inspect the RSA parameters

The challenge provides three RSA ciphertexts encrypted with the same public exponent:

```text
e = 3
```

The ciphertexts correspond to the same message but use different RSA moduli.

This setup suggests Håstad's broadcast attack.

### Step 2: Understand the attack

When the same message is encrypted with a small public exponent and sent to multiple recipients, the Chinese Remainder Theorem (CRT) can combine the ciphertexts.

For three recipients using exponent 3, the combined result can reveal:

```text
m³
```

An exact integer cube root then recovers the original message.

### Step 3: Check for the short-message case

In this challenge, the ciphertext is sufficiently small relative to the modulus that the message's cube is smaller than the modulus:

```text
m³ < n
```

Therefore, RSA modular reduction does not occur for that ciphertext.

This means the ciphertext is already the ordinary integer cube of the original message. Taking its exact integer cube root directly recovers the message.

### Step 4: Recover the flag

Calculate the integer cube root of the ciphertext and convert the resulting integer into bytes.

The decoded plaintext reveals the flag.

The full CRT approach also produces the same result.

## Flag

`CyberZerk{Hastad_br0adcast_3_d3st1nat41r3s_1_m3ss4g3}`
