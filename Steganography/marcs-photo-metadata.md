# Marc's Photo Metadata

**Challenge title:** Trombinoscope  
**Category:** Steganography  
**Difficulty:** Medium  
**Points:** 87  
**Author:** Perceval

## File

- `trombi_marc_v.jpg`

## Solution

### Step 1: Understand the challenge

The hint says that a photo is never just an image and asks us to look at what it carries with it.

This points to **metadata**, which is additional information stored inside an image file.

### Step 2: Open the image in Aperi'Solve

1. Visit [Aperi'Solve](https://www.aperisolve.com/).
2. Upload `trombi_marc_v.jpg`.
3. Check the metadata / EXIF information in the results.
4. Look for the `UserComment` field.

### Step 3: Find the hidden flag

Inside the `UserComment` field, we find the hidden flag:

```text
CyberZerk{M3t4d0nn33s_EXIF_B4v4rd3s}
```

The `UserComment` field contains the flag after an `ASCII` prefix.

## Flag

`CyberZerk{M3t4d0nn33s_EXIF_B4v4rd3s}`
