# Expense Report

**Challenge title:** Note de Frais  
**Category:** Cryptography  
**Difficulty:** Easy  
**Points:** 75  
**Author:** Perceval

## File

- `note_chiffree.txt`

## Solution

### Step 1: Decode the Base64 content

The file contains Base64-encoded data. Decode it:

```bash
base64 -d note_chiffree.txt
```

The decoded output begins with:

```text
HmmetDjfv{Pt1q3x_QZDKV_...}
```

The result resembles a flag, but the content is still encrypted.

### Step 2: Identify the cipher

Trying all 26 Caesar shifts does not produce readable text.

The expected flag format starts with `CyberZerk`. Comparing the first nine ciphertext letters with the expected flag prefix gives these shifts:

```text
5, 14, 11, 0, 2, 4, 5, 14, 11
```

These shifts correspond to the repeating key:

```text
FOLACE
```

This identifies the cipher as Vigenère.

### Step 3: Decrypt the message

Use the key `folace` to decrypt the Base64-decoded text.

The Vigenère decryption operation is:

```text
Plaintext = (Ciphertext - Key) mod 26
```

The recovered plaintext is the flag.

## Flag

`CyberZerk{Pr1m3s_CODIR_P3nd4nt_L3s_R3fus_D3_C0ng3s}`
