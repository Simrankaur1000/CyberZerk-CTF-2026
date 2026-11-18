# Double Bottom

**Challenge title:** Double Fond  
**Category:** Steganography  
**Difficulty:** Hard  
**Points:** 175  
**Author:** Perceval

## File

- `affiche_crise.png`

## Solution

### Step 1: Understand the challenge

The hint says that the image hides two things:

- A key stored inside the image.
- A hidden message attached after the image.

The key is needed to decode the message.

### Step 2: Check the PNG metadata

A PNG file contains different chunks that store image information and metadata.

The `tEXt` chunk contains a field named `cle` (French for key).

Its value is:

```text
Y3IxczMtdGMyNg==
```

### Step 3: Decode the key

The value looks like Base64. Decode it to get the key:

```text
cr1s3-tc26
```

### Step 4: Find the hidden message

The PNG image ends at byte 13,056, but the actual file is 13,091 bytes.

This means there are **35 extra bytes** after the PNG's `IEND` chunk.

These extra bytes contain the encrypted message.

### Step 5: Decode the message using XOR

The hidden message is encrypted using repeating-key XOR.

Use Python to decode it:

```python
import base64

d = open("affiche_crise.png", "rb").read()

key = base64.b64decode("Y3IxczMtdGMyNg==")

msg = d[13056:]

decoded = bytes(b ^ key[i % len(key)] for i, b in enumerate(msg))

print(decoded.decode())
```

The script extracts the extra bytes and XORs them with the repeating key.

### Step 6: Recover the flag

The decoded message reveals:

```text
CyberZerk{St3g0_Pu1s_X0R_B13n_J0u3}
```

## Flag

`CyberZerk{St3g0_Pu1s_X0R_B13n_J0u3}`
