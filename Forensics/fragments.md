# Fragments

**Challenge title:** Fragments  
**Category:** Forensics  
**Points:** 150  
**Difficulty:** Hard  
**Author:** Perceval

## Challenge

The challenge provides a raw piece of disk space. It has no file table, so the original filenames and folders are unavailable. The goal is to find and recover the hidden file.

This technique is called **file carving**. It means recovering a file by recognising its contents and file signature.

## Files

- `secteur.raw` — raw disk fragment

## Solution

### 1. Check the raw file

Run:

```bash
file secteur.raw
xxd secteur.raw | head
```

The file command reports generic data. The beginning contains random-looking bytes, so the hidden file does not start at the beginning of the raw fragment.

### 2. Search for readable text

Run:

```bash
strings secteur.raw
```

The output contains strings such as `tEXt`, `iTXt`, `IDAT`, and `Commentaire`. These are names used by PNG image files, which suggests that a PNG is hidden in the raw data.

### 3. Find the PNG signature

Every PNG starts with this 8-byte signature:

```
89 50 4E 47 0D 0A 1A 0A
```

Searching for this signature finds the start of the PNG at **byte offset 5000**.

### 4. Check the PNG chunks

PNG files contain chunks. Each chunk has a length, a type, data, and a CRC checksum.

The chunks found in this image are:

| Offset | Chunk | Contents |
| --- | --- | --- |
| 5008 | `IHDR` | Image size: 64 × 64 pixels |
| 5033 | `iTXt` | Comment: "Capture interne — ne pas diffuser" |
| 5096 | `tEXt` | Contains the flag |
| 5160 | `IDAT` | Image data |
| 5277 | `IEND` | End of the PNG |

All five CRC checksums are valid, confirming that the PNG data is intact.

### 5. Extract the PNG

The PNG is 289 bytes long. Extract the data starting at byte 5000 and ending after its `IEND` chunk, then save it as `carved.png`.

The extracted file is recognised as a valid PNG. The image itself does not display the flag; the flag is stored in its metadata.

### 6. Read the flag from the metadata

The PNG's `tEXt` chunk has the keyword `flag`, followed by the flag value.

## Flag

```
CyberZerk{c4rv1ng_l3_PNG_surv1t_a_l_3ff4c3m3nt}
```

## What I learned

A deleted file may still exist in raw disk space. Even when its filename and file table entry are gone, its file signature and internal structure can help recover it.