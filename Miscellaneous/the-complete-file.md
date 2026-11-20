# The Complete File

**Challenge title:** Le Dossier Complet  
**Category:** Miscellaneous  
**Difficulty:** Extreme  
**Points:** 302  
**Author:** Perceval

## File

- `dossier.txt`

## Solution

### Step 1: Identify the three required challenges

The challenge requires the first keyword (the first segment before `_`) from the flags of three previously solved challenges.

| Piece | Challenge | Keyword |
|---|---|---|
| A | Diffusion Imprudente | `Hastad` |
| B | Export XML | `XXE` |
| C | Fragments | `c4rv1ng` |

### Step 2: Combine the keywords

Combine the keywords in the specified order, separated by `|`:

```text
Hastad|XXE|c4rv1ng
```

### Step 3: Generate the SHA-256 hash

Enter the combined string into a SHA-256 hash generator and take the first 12 hexadecimal characters of the result.

```text
a2a34d87d580
```

## Flag

`CyberZerk{DOSSIER_c0mpl3t_a2a34d87d580}`

## Security Lesson

This challenge demonstrates how information collected from multiple challenges can be combined and processed using a cryptographic hash to obtain a final flag.
