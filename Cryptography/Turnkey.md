# Turnkey

**Challenge title:** Clés en Main  
**Category:** Cryptography  
**Difficulty:** Hard  
**Points:** 175  
**Author:** Perceval

## File

- Challenge-provided RSA parameters and ciphertext

## Solution

### Step 1: Identify the weakness

The challenge hint suggests that the two RSA prime factors are very similar or close to each other.

This indicates a possible weakness in RSA key generation.

When the primes `p` and `q` are sufficiently close, Fermat's factorization method can be used to factor the RSA modulus.

### Step 2: Apply Fermat factorization

The RSA modulus is:

```text
n = p × q
```

Fermat's method searches for integers `a` and `b` satisfying:

```text
n = a² - b²
```

Starting with:

```text
a = ceil(sqrt(n))
```

Increase `a` until:

```text
a² - n = b²
```

where `b²` is a perfect square.

The two prime factors can then be calculated:

```text
p = a - b
q = a + b
```

The factorization converges quickly because the two primes are close.

### Step 3: Recover the private key

Once `p` and `q` are recovered, calculate Euler's totient:

```text
φ(n) = (p - 1) × (q - 1)
```

The private exponent is:

```text
d = e⁻¹ mod φ(n)
```

This is the modular multiplicative inverse of the public exponent.

### Step 4: Decrypt the ciphertext

Use the recovered private exponent to decrypt the RSA ciphertext:

```text
m = c^d mod n
```

Convert the resulting integer into bytes to obtain the original plaintext.

The decrypted message reveals the flag.

## Flag

`CyberZerk{F3rm4t_C4ss3_L3s_Pr3m13rs_Tr0p_Pr0ch3s}`
