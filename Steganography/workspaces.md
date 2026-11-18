# Workspaces

**Challenge title:** Espaces de Travail  
**Category:** Steganography  
**Difficulty:** Medium  
**Points:** 100  
**Author:** Perceval

## File

- `note.txt`

## Solution

### Step 1: Understand the challenge

The hint says that the harmless note "weighs more than its text".

This suggests that the file contains hidden information that is not visible when reading it normally.

The unusual gaps after the word `Note` are a clue. They may contain invisible Unicode characters.

### Step 2: Find the invisible characters

The file contains two types of zero-width Unicode characters:

| Character | Unicode | Count |
|---|---|---:|
| Zero Width Space | `U+200B` | 144 |
| Zero Width Non-Joiner | `U+200C` | 168 |

There are 312 invisible characters in total.

### Step 3: Understand the encoding

There are two different invisible characters, so they can represent binary values:

- `U+200B` = `0`
- `U+200C` = `1`

Since 312 bits make 39 bytes, we can convert the binary data into readable text.

### Step 4: Decode the hidden message

Use Python to extract and decode the invisible characters:

```python
t = open("note.txt", encoding="utf-8").read()

z = [c for c in t if c in "\u200b\u200c"]

bits = "".join("0" if c == "\u200b" else "1" for c in z)

print(bytes(int(bits[i:i+8], 2) for i in range(0, len(bits), 8)).decode())
```

The decoded text reveals the hidden flag.

### Step 5: Recover the flag

```text
CyberZerk{z3r0_w1dth_c_3st_pas_vis1bl3}
```

## Flag

`CyberZerk{z3r0_w1dth_c_3st_pas_vis1bl3}`
