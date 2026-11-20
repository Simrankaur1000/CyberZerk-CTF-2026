# Second Factor

**Challenge title:** Second Facteur  
**Category:** Awareness  
**Difficulty:** Easy  
**Points:** 75  
**Author:** Perceval

## File

- `second-facteur-README.txt`

## Solution

### Step 1: Collect the TOTP parameters

The README provides the following TOTP configuration:

| Parameter | Value |
|---|---|
| Algorithm | HMAC-SHA1 |
| Step | 30 seconds |
| Digits | 6 |
| Secret (Base32) | `JBSWY3DPEHPK3PXP` |
| Time | `1762900000` |

### Step 2: Compute the time counter

According to RFC 6238, calculate the counter by dividing the Unix timestamp by the 30-second time step:

```text
counter = floor(T / 30)
        = floor(1762900000 / 30)
        = 58763333
```

### Step 3: Compute the HMAC

Decode the Base32 secret and calculate HMAC-SHA1 using the counter as an 8-byte big-endian integer.

The resulting HMAC digest is:

```text
2c0d7063c651851cb50d4efd9cb7fc1f2d29412f
```

### Step 4: Apply dynamic truncation

Follow the HOTP dynamic truncation process defined in RFC 4226:

1. Calculate the offset using the last byte's lower four bits: `0x2f & 0x0f = 15`.
2. Extract four bytes starting at offset 15: `1f 2d 29 41`.
3. Clear the most significant bit: `0x1f2d2941`.
4. Convert the result to decimal: `523053377`.
5. Keep the last six digits: `053377`.

The resulting TOTP code is `053377`.

### Step 5: Verify using Python

```python
import base64
import hmac
import hashlib
import struct

key = base64.b32decode("JBSWY3DPEHPK3PXP")
T = 1762900000

h = hmac.new(key, struct.pack(">Q", T // 30), hashlib.sha1).digest()
o = h[-1] & 0xf

code = (struct.unpack(">I", h[o:o+4])[0] & 0x7fffffff) % 10**6

print(f"{code:06d}")
```

Output:

```text
053377
```

The implementation also reproduces the RFC 6238 test vector: `T = 59` produces `287082`.

## Flag

`CyberZerk{2fa_053377}`

## Security Lesson

A second factor protects accounts against attacks involving stolen passwords, but the TOTP secret must also be protected.

Never share TOTP secrets in READMEs, chats, or screenshots. Prefer phishing-resistant authentication methods such as FIDO2 and passkeys where possible, since attackers can still relay TOTP codes through fake login pages.
