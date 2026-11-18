# A Facade Smile

**Challenge title:** Sourire de façade  
**Category:** Steganography  
**Difficulty:** Easy  
**Points:** 100  
**Author:** Perceval

## File

- `motivation.png`

## Solution

### Step 1: Understand the challenge

The hint says to look beyond appearances. This suggests that the image may contain hidden data that is not visible when we simply open it.

### Step 2: Open the image in Aperi'Solve

1. Visit [Aperi'Solve](https://www.aperisolve.com/).
2. Upload `motivation.png`.
3. Wait for the analysis to finish.
4. Open the **Binwalk** results.

Binwalk detects a hidden ZIP archive attached to the image.

### Step 3: Extract the hidden file

1. Download the ZIP archive from the Binwalk results.
2. Extract the ZIP file. No password is required.
3. Open `journal_intime.txt`.

### Step 4: Find the flag

At the end of the text file, we find the hidden flag:

```text
CyberZerk{S0ur1r3_Pour_C4ch3r_L3_Burn0ut}
```

## Flag

`CyberZerk{S0ur1r3_Pour_C4ch3r_L3_Burn0ut}`
