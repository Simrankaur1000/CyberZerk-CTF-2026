# The CFO's Vault

**Challenge title:** Le Coffre du DAF  
**Category:** Cryptography  
**Difficulty:** Medium  
**Points:** 100  
**Author:** Perceval

## File

- Challenge-provided hexadecimal ciphertext file

## Solution

### Step 1: Inspect the ciphertext

The provided hexadecimal ciphertext represents 40 bytes.

The challenge hint suggests that the same key is reused repeatedly. This points to repeating-key XOR encryption.

### Step 2: Recover the XOR key

The expected flag format starts with:

```text
CyberZerk{
```

This known plaintext can be XORed with the first ciphertext bytes to recover the corresponding key bytes.

The recovered repeating key is:

```text
TRICATEL
```

The key is 8 bytes long and repeats throughout the ciphertext.

### Step 3: Decrypt the ciphertext

XOR each ciphertext byte with the corresponding repeating key byte.

The operation is:

```text
Plaintext[i] = Ciphertext[i] XOR Key[i % Key_Length]
```

Using the recovered key `TRICATEL` decrypts the complete ciphertext and reveals the flag.

## Flag

`CyberZerk{X0R_R3p3t3_C_3st_P4s_S3cur1s3}`
