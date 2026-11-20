# Military Encryption

**Challenge title:** Le Chiffrement Militaire  
**Category:** Cryptography  
**Difficulty:** Intro  
**Points:** 100  
**Author:** Perceval

## File

- Challenge-provided encoded text

## Solution

### Step 1: Identify Base64 encoding

The provided token ends with `==`, which is a common Base64 padding pattern.

This suggests that the data is Base64-encoded.

### Step 2: Decode the first layer

Decode the original token:

```bash
base64 -d input.txt
```

The output begins with:

```text
UTNs...
```

This is another indication of Base64 encoding.

### Step 3: Decode the second layer

Decode the newly obtained output again:

```bash
base64 -d layer1.txt
```

The result begins with:

```text
Q3li...
```

It is still Base64-encoded.

### Step 4: Decode the final layer

Perform one more Base64 decoding operation:

```bash
base64 -d layer2.txt
```

The final decoded text reveals the flag.

## Flag

`CyberZerk{B4s364_N3st_P4s_Du_Ch1ffr3m3nt}`
