# Session Token

**Challenge title:** Le Jeton de Session  
**Category:** Cryptography  
**Difficulty:** Hard  
**Points:** 175  
**Author:** Perceval

## File

- `tokens.txt`
- `secret.bin`

## Solution

### Step 1: Identify the weak random number generator

The challenge provides 624 consecutive outputs from Python's `random` module.

Python uses the Mersenne Twister (MT19937) algorithm, which is not cryptographically secure.

Its internal state consists of 624 32-bit words. Therefore, collecting 624 consecutive outputs is enough to reconstruct the generator's internal state.

### Step 2: Recover the internal state

Each MT19937 output undergoes a tempering transformation before being returned.

Reverse the four tempering operations in reverse order:

1. Undo `y ^= y >> 18`.
2. Undo `y ^= (y << 15) & 0xefc60000`.
3. Undo `y ^= (y << 7) & 0x9d2c5680`.
4. Undo `y ^= y >> 11`.

Applying these operations to all 624 leaked outputs recovers the original internal state words.

### Step 3: Clone the generator

Create a new Python `random.Random` instance and load the recovered state:

```python
rng.setstate((3, tuple(state + [624]), None))
```

The index value `624` forces the generator to perform a twist operation before producing its next output.

This allows the cloned generator to predict the server's subsequent outputs.

### Step 4: Reconstruct the keystream

The leaked values are not the values used to encrypt the HR note. The encryption uses the following outputs generated after the leaked sequence.

Pack the predicted 32-bit integers as big-endian values:

```python
struct.pack(">I", value)
```

Combine the resulting bytes into the predicted keystream.

### Step 5: Recover the plaintext

XOR the reconstructed keystream with `secret.bin`.

The resulting plaintext reveals the hidden message and the flag.

## Flag

`CyberZerk{MT19937_s3_cl0n3_av3c_624_s0rt13s_untemper}`

## Security Lesson

Never use Python's `random` module or MT19937 for security-sensitive operations. Use the `secrets` module for cryptographically secure random values.
