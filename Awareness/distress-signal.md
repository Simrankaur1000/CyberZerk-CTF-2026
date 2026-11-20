# Distress Signal

**Challenge title:** Signal de Détresse  
**Category:** Awareness  
**Difficulty:** Intro  
**Points:** 100  
**Author:** Perceval

## File

- No separate file. The encoded message is provided in the challenge description.

## Solution

### Step 1: Identify the encoding

The challenge contains the following encoded message:

```text
Q3liZXJaZXJre0xlX2J1cm5fb3V0X25lc3RfcGFzX3VuZV9mYWlibGVzc2V9
```

It uses the Base64 character set and has a length compatible with Base64 encoding.

The beginning of the string, `Q3liZXJaZXJr`, also corresponds to the Base64 encoding of `CyberZerk`.

This identifies the encoding as Base64.

### Step 2: Decode the message

Use the following shell command:

```bash
echo 'Q3liZXJaZXJre0xlX2J1cm5fb3V0X25lc3RfcGFzX3VuZV9mYWlibGVzc2V9' | base64 -d
```

Alternatively, use Python:

```python
import base64

encoded = "Q3liZXJaZXJre0xlX2J1cm5fb3V0X25lc3RfcGFzX3VuZV9mYWlibGVzc2V9"

print(base64.b64decode(encoded).decode())
```

The decoded result is:

```text
CyberZerk{Le_burn_out_nest_pas_une_faiblesse}
```

It means "Burnout is not a weakness."

## Flag

`CyberZerk{Le_burn_out_nest_pas_une_faiblesse}`

## Security Lesson

Base64 is an encoding, not encryption. Anyone can decode it without a secret key.

The deeper message concerns burnout. Prolonged exhaustion can affect both people and security, as tired engineers are more likely to make mistakes. Recognizing the warning signs and seeking support are important.
