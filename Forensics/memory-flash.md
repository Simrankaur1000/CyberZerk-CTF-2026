# Memory Flash

**Challenge title:** Vidage de Mémoire  
**Category:** Forensics  
**Difficulty:** Extreme  
**Points:** 225  
**Author:** Perceval

## File

- `heap.dump` — memory dump from the HR authentication service.

## Solution

### 1. Check the memory dump

Run:

```bash
file heap.dump
strings heap.dump
```

The file is 20,593 bytes and is identified only as generic data. The strings output contains mostly random-looking data, but two useful markers appear:

- `AUTHBLK` at offset `0x2000`
- `LOGBUF` at offset `0x3045`

### 2. Check the log buffer

The data after `LOGBUF` looks like Base64. Decoding it gives:

```
CyberZerk{decoy-anc13n_jet0n_d3_t3st}
```

This is a decoy flag. The challenge points us towards the `AUTHBLK` structure, so do not submit this value.

### 3. Read the AUTHBLK structure

At offset `0x2000`, the structure begins with:

```
41 55 54 48 42 4c 4b 00 5a 2a 00 ...
 A  U  T  H  B  L  K \\0  key len reserved
```

The structure is laid out as follows:

| Bytes | Meaning |
| --- | --- |
| `AUTHBLK\\0` | Structure marker |
| `5A` | XOR key |
| `2A` | Token length: 42 bytes |
| `00` | Reserved byte |
| Next 42 bytes | Obscured token |

### 4. Understand the encoding

The token bytes are XORed with `0x5A`.

For example, the underscore character has the hexadecimal value `0x5F`. Applying XOR with `0x5A` gives `0x05`.

To recover the original data, XOR each stored byte with the same key, `0x5A`.

### 5. Decode the token

The first few bytes demonstrate the process:

| Stored byte | XOR with `5A` | Character |
| --- | --- | --- |
| `19` | `43` | C |
| `23` | `79` | y |
| `38` | `62` | b |
| `3F` | `65` | e |
| `28` | `72` | r |
| `00` | `5A` | Z |
| `3F` | `65` | e |
| `28` | `72` | r |
| `31` | `6B` | k |
| `21` | `7B` | { |

Continue for all 42 bytes. The decoded text matches the length stored in the structure.

## Flag

```
CyberZerk{c0r3_dump_struct_x0r_auth_bl0ck}
```
