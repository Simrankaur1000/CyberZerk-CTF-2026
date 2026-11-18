# Legacy Macro

**Challenge title:** Macro Héritée  
**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 125  
**Author:** Perceval

## Files

- `macro.vba` — contains the VBA macro code.
- `README.txt` — contains the challenge instructions.

## Solution

### 1. Understand the challenge

The challenge asks what a suspicious macro displays when a workbook is opened.

In VBA, a procedure named `Workbook_Open()` runs automatically when the workbook opens. We need to understand what the macro does without running it.

### 2. Read the macro

Open `macro.vba` with Notepad or another text editor.

The macro uses four steps to hide its message:

1. `Chr()` converts numbers into characters.
2. Base64 decoding turns the encoded text into another string.
3. `StrReverse` reverses that string.
4. `MsgBox` displays the final message in a pop-up window.

### 3. Convert the Chr() values

The macro contains several `Chr()` functions. Each number represents a character.

For example:

- `Chr(102)` produces `f`.
- `Chr(84)` produces `T`.
- `Chr(78)` produces `N`.

Joining all 72 characters gives:

```text
fTNzcjN2M3JfNDZiX3JoY18zbmk0aGNfbjAxdDRjc3VmYjBzM2RfYWJ2e2tyZVpyZWJ5Qw==
```

The ending `==` is a clue that the text is Base64 encoded.

### 4. Decode the Base64 text

After Base64 decoding, we get:

```text
}3sr3v3r_46b_rhc_3ni4hc_n01t4csufb0s3d_abv{kreZrebyC
```

The text appears backwards because the macro applies another operation.

### 5. Reverse the text

The VBA function `StrReverse` reverses the entire string.

After reversing it, we get:

```text
CyberZerk{vba_d3s0bfusc4t10n_ch4in3_chr_b64_r3v3rs3}
```

This is the message displayed by the macro.

## Flag

```text
CyberZerk{vba_d3s0bfusc4t10n_ch4in3_chr_b64_r3v3rs3}
```

**In simple words:** The macro hides its message using number-to-character conversion, Base64 decoding, and reversing the text. It then displays the recovered message using a pop-up.
