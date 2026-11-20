# Load Plan

**Challenge title:** Plan de Charge  
**Category:** Steganography  
**Difficulty:** Hard  
**Points:** 175  
**Author:** Perceval

## File

- `plan_de_charge.png`

## Solution

### Step 1: Understand the challenge

The hint says that the message is hidden in the **Least Significant Bits (LSB)** of the image pixels.

However, the bits are not stored in the usual sequential order.

The challenge provides a seed of `1337` and a message length of `47` bytes.

### Step 2: Understand the encoding

The image is a 320 × 200 RGB PNG.

The hidden data is stored in the LSB of the **blue colour channel**.

A 47-byte message contains:

```text
47 × 8 = 376 bits
```

### Step 3: Recover the shuffled pixel order

The seed `1337` is used with NumPy's `default_rng()` to generate a shuffled sequence of pixel positions.

We must follow this sequence instead of reading the pixels from left to right.

### Step 4: Extract the hidden message

Use Python with Pillow and NumPy:

```python
from PIL import Image
import numpy as np

img = np.array(Image.open("plan_de_charge.png").convert("RGB"))

blue = img[:, :, 2] & 1

positions = np.random.default_rng(1337).permutation(blue.size)[:376]

bits = blue.flatten()[positions]

data = np.packbits(bits).tobytes()

print(data.decode())
```

The script extracts 376 blue-channel LSBs in the seeded shuffled order and converts them into readable text.

### Step 5: Recover the flag

The extracted message is:

```text
CyberZerk{lsb_d1sp3rs3_p4r_prng_pas_s3qu3nt13l}
```

## Flag

`CyberZerk{lsb_d1sp3rs3_p4r_prng_pas_s3qu3nt13l}`
