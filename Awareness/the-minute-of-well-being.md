# The Minute of Well-being

**Challenge title:** La Minute Bien-être  
**Category:** Awareness  
**Difficulty:** Intro  
**Points:** 50  
**Author:** Perceval

## File

- `minute_bienetre.txt`

## Solution

### Step 1: Read the thread

The exported channel contains two types of messages.

The Direction and Manager messages provide the context: forced fun, guilt about sick leave, statements such as "smiling = commitment", and unpaid overtime presented as team spirit.

The final messages come from `Marc_V`, who says he can no longer communicate in clear text and leaves the following string:

```text
PloreMrex{E3c3e3_Yn_Z4avchy4g10a_E3fg3_Uhz41a}
```

### Step 2: Identify the method

Marc mentions that the method is "not an expert code, just a shift".

The prefix `PloreMrex` has the same length as `CyberZerk`. Comparing the letters shows a shift of 13 positions. Digits and symbols remain unchanged.

This identifies the cipher as **ROT13**, a Caesar cipher with a shift of 13.

### Step 3: Decode the message

Using Python:

```python
import codecs

print(codecs.decode(
    "PloreMrex{E3c3e3_Yn_Z4avchy4g10a_E3fg3_Uhz41a}",
    "rot13"
))
```

Alternatively, use a shell command:

```bash
echo 'PloreMrex{E3c3e3_Yn_Z4avchy4g10a_E3fg3_Uhz41a}' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

The decoded result is:

```text
CyberZerk{R3p3r3_La_M4nipul4t10n_R3st3_Hum41a}
```

It means: "Spot the manipulation, stay human."

## Flag

`CyberZerk{R3p3r3_La_M4nipul4t10n_R3st3_Hum41n}`

## Security Lesson

ROT13 is obfuscation, not encryption. It has no key and can be reversed easily.

The more important lesson is recognizing workplace manipulation. Mandatory "fun", guilt-tripping about sick leave, and treating complaints as disloyalty are warning signs of an unhealthy work environment.
