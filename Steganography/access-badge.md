# Access Badge

**Challenge title:** Badge d'Accès

**Category:** Steganography

**Points:** 62

**Difficulty:** Easy

**Author:** Perceval

## File

- `badge_marc_v.png`

## Solution

### Step 1: Understand the challenge

The challenge provides a photo of Marc V.'s employee badge. HR claims that the image proves he was present. We need to find out what information is hidden inside the image.

The image has a suspicious dark strip near the bottom, suggesting that some information may be hidden in its pixels.

### Step 2: Check the image

The image is a 480 × 300 PNG file in RGB format.

There is no useful text metadata or extra data appended after the PNG. This suggests that the hidden information is stored inside the image pixels.

### Step 3: Understand LSB steganography

LSB stands for Least Significant Bit. It is the last bit of a colour value.

Changing this bit changes the colour value by only 1, making the modification almost impossible to notice.

This technique can be used to hide messages inside images.

### Step 4: Extract the hidden message

The hidden message is stored in the lowest bits of the blue colour channel.

I used Python with Pillow and NumPy to extract the blue-channel LSBs and convert them into readable text.

```python
from PIL import Image
import numpy as np

im = np.array(Image.open("badge_marc_v.png").convert("RGB"))

bits = im[:, :, 2] & 1
data = np.packbits(bits.reshape(-1)).tobytes()

start = data.find(b"CyberZerk{")
end = data.find(b"}", start)

if start != -1 and end != -1:
    print(data[start:end + 1].decode())
else:
    print("Flag not found")
```

### Step 5: Recover the flag

The extracted data reveals the hidden flag:

```text
CyberZerk{B4dg3_N0cturn3_H3ur3s_C4ch33s}
```

## Flag

`CyberZerk{B4dg3_N0cturn3_H3ur3s_C4ch33s}`
