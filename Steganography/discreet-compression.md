# Discreet Compression

**Challenge title:** Compression Discrète  
**Category:** Steganography  
**Difficulty:** Hard  
**Points:** 200  
**Author:** Perceval

## File

- `compression-discrete-photo.jpg`

## Solution

### Step 1: Understand the challenge

The hint says that the hidden message does not survive in the pixels but in the compression.

This points to the JPEG's **Discrete Cosine Transform (DCT) coefficients**. JPEG compression stores quantized frequency coefficients, which can be used to hide information.

### Step 2: Inspect the JPEG file

First, check the file information and metadata:

```bash
file compression-discrete-photo.jpg
exiftool compression-discrete-photo.jpg
```

The image is a 480 × 360 baseline JPEG.

The metadata does not reveal any hidden message.

Next, check for appended data:

```bash
binwalk compression-discrete-photo.jpg
```

No additional data was found after the JPEG end marker (`FFD9`).

### Step 3: Decode the DCT coefficients

Normal image libraries such as Pillow and OpenCV return decoded pixels, not the original quantized DCT coefficients.

Therefore, a custom baseline-JPEG Huffman decoder was used to extract the coefficients directly.

The decoder:

1. Parses the JPEG Huffman tables.
2. Decodes DC differences and AC run/size symbols.
3. Reconstructs the quantized coefficients for each 8 × 8 block.
4. Identifies the luminance (Y) component.

The image uses 4:2:0 chroma subsampling, and 4,140 blocks were decoded.

### Step 4: Extract the hidden bits

The challenge uses the classic **JSteg** technique.

The message is hidden in the LSBs of eligible AC coefficients. Coefficients with values `0`, `1`, and `-1` are skipped.

The extraction logic is:

```python
bits = []

for comp_id, coeffs in blocks:
    if comp_id != 1:
        continue

    for k in range(1, 64):
        v = coeffs[k]

        if v in (0, 1, -1):
            continue

        bits.append(v & 1)
```

Only the luminance (Y) component is used.

### Step 5: Convert the bits into text

The extracted bits are packed into bytes using LSB-first ordering:

```python
out = bytearray()

for i in range(0, len(bits) - 7, 8):
    out.append(sum(bits[i + t] << t for t in range(8)))

print(out)
```

The output begins with:

```text
jsteg.\x00\x00\x00CyberZerk{...}
```

The `jsteg.` prefix and three null bytes appear before the hidden flag.

### Step 6: Recover the flag

The extracted message reveals:

```text
CyberZerk{jst3g_lsb_d3s_c03ff1c13nts_dct_jp3g}
```

## Flag

`CyberZerk{jst3g_lsb_d3s_c03ff1c13nts_dct_jp3g}`
