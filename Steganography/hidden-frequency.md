# Hidden Frequency

**Challenge title:** Fréquence Cachée  
**Category:** Steganography  
**Difficulty:** Easy  
**Points:** 75  
**Author:** Perceval

## File

- `message.wav`

## Solution

### Step 1: Understand the challenge

The challenge provides an audio file that sounds like noise.

The hint says: "The ear hears only noise... But the eye?"

This suggests that the hidden message is meant to be seen rather than heard. We can use a **spectrogram** to display the audio as an image and reveal hidden text.

### Step 2: Open the audio in Audacity

1. Open Audacity.
2. Drag `message.wav` into the window.
3. Click the small dropdown arrow beside the audio track name.
4. Select **Spectrogram**.

The audio waveform changes into a visual representation of its frequencies.

### Step 3: Adjust the spectrogram settings

If the letters are difficult to read:

1. Open the track dropdown menu again.
2. Select **Spectrogram Settings**.
3. Adjust these settings:
   - Window size: **1024** or **2048**
   - Maximum frequency: **8000 Hz**
   - Scale: **Linear**
4. Make the track taller and zoom in if needed.

The hidden letters should become visible in the spectrogram.

### Step 4: Read the hidden message

The letters appear between approximately 1,500 and 3,000 Hz.

Reading them from left to right gives:

```text
CyberZerk{sp3ctr0_wav}
```

### Step 5: Identify the confusing character

The character after `sp` may initially look like either `B` or `3`.

Zooming in with a shorter analysis window makes it easier to distinguish them. The character has an open left side, so it is `3`, not `B`.

The complete message is therefore `CyberZerk{sp3ctr0_wav}`.

## Flag

`CyberZerk{sp3ctr0_wav}`
