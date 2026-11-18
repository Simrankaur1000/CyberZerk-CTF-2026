# Catches

**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 125  
**Author:** Perceval

## Files

- `frappes.pcap` — the USB keyboard traffic capture.
- `README.txt` — the challenge description.

## Solution

### 1. Understand the challenge

The capture contains data from a USB keyboard connected to a computer. Our task is to recover the text that was typed.

### 2. Open the capture

1. Open Wireshark.
2. Select **File → Open**.
3. Choose `frappes.pcap`.
4. The capture contains 86 packets.

The file records USB traffic, not ordinary network traffic.

### 3. Understand the keyboard reports

The keyboard sends small reports that describe which keys are pressed. Each report in this capture has 8 bytes.

For example:

```text
02 00 06 00 00 00 00 00
```

The important parts are:

- The first byte, `02`, means the Shift key is pressed.
- The third byte, `06`, represents the `c` key.
- Together, they produce an uppercase `C`.

An all-zero report means that no key is pressed. These reports can be ignored when rebuilding the text.

### 4. Convert the key codes

The key codes are hexadecimal values. Match them with the standard USB keyboard (HID) key table.

Some examples from this capture:

| Key code | Character |
|---|---|
| `06` | c |
| `1c` | y |
| `05` | b |
| `08` | e |
| `15` | r |
| `1d` | z |
| `2d` | - |
| `2f` | [ |
| `30` | ] |

The Shift key changes some characters:

- Shift + `c` = `C`
- Shift + `z` = `Z`
- Shift + `-` = `_`
- Shift + `[` = `{`
- Shift + `]` = `}`

### 5. Reconstruct the typed text

Read the non-empty keyboard reports in order and convert each key code into its character. For example, the first reports produce:

| Key press | Character |
|---|---|
| Shift + `06` | C |
| `1c` | y |
| `05` | b |
| `08` | e |
| `15` | r |
| Shift + `1d` | Z |

Continue the same process for the remaining reports. The complete text is:

```text
CyberZerk{usb_h1d_k3yb0ard_r3p0rt_8_0ct3ts}
```

## Flag

```text
CyberZerk{usb_h1d_k3yb0ard_r3p0rt_8_0ct3ts}
```

The flag refers to USB HID keyboard reports, which use 8-byte reports to describe key presses.

**Note:** The full packet-by-packet decoding was checked from the capture. Wireshark is used to open and inspect the USB packets; it does not automatically turn this capture into readable text.
