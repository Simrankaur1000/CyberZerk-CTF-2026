# The Inner Voice

**Challenge title:** La Voix Intérieure  
**Category:** Cryptography  
**Difficulty:** Intro  
**Points:** 100  
**Author:** Perceval

## File

- Challenge-provided encrypted text

## Solution

### Step 1: Identify the cipher

The challenge describes a secular substitution method attributed to a 16th-century French diplomat.

This clue points to the Vigenère cipher, named after Blaise de Vigenère.

### Step 2: Identify the key

The challenge description repeatedly mentions the phrase:

```text
je suis nul
```

This phrase is used as the encryption key:

```text
jesuisnul
```

Spaces are removed before using the phrase as the key.

### Step 3: Decrypt the ciphertext

The encrypted text is:

```text
LctyzRrlv{Cy_Sm_Bs_Cflli_Awq}
```

Decrypt it using the Vigenère cipher with the key `jesuisnul`.

The resulting plaintext reveals the flag.

## Flag

`CyberZerk{Tu_As_Ta_Place_Ici}`
