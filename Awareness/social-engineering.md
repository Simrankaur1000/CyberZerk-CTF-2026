# Social Engineering

**Challenge title:** Ingénierie Sociale  
**Category:** Awareness  
**Difficulty:** Medium  
**Points:** 125  
**Author:** Perceval

## File

- `messages_internes.zip`
  - `mail_fournisseur.txt`
  - `notes_standard.txt`
  - `chat_support.txt`

## Solution

### Step 1: Reconstruct the attack chain

The three files describe a multi-stage social engineering attack targeting Marc.

Arrange the events chronologically:

| Stage | File | Attacker persona | Time |
|---|---|---|---|
| 1 | `mail_fournisseur.txt` | Fake supplier sending an unpaid invoice from `facturation@tricatel-fournisseur[.]ru` | 09 Nov, 09:12 |
| 2 | `notes_standard.txt` | Fake IT technician calling the switchboard and creating urgency | 09 Nov, 11:47 |
| 3 | `chat_support.txt` | Fake maintenance agent installed on Marc's computer | 09 Nov, 16:03 |

Together, these stages demonstrate pretexting, authority and urgency pressure, and finally remote access to the victim's machine.

### Step 2: Extract the three fragments

Each stage contains a fragment of the flag, encoded using a different method.

**Fragment 1 — Base64**

The supplier email contains a value labelled "reference de dossier":

```text
Q3liZXJaZXJre0NoNDFu
```

Decode it:

```python
import base64

f1 = base64.b64decode("Q3liZXJaZXJre0NoNDFu").decode()
print(f1)
```

Result:

```text
CyberZerk{Ch41n
```

**Fragment 2 — ROT13**

The switchboard note contains a value labelled "code de maintenance":

```text
3_1at3a13e13_F0
```

Apply ROT13:

```python
import codecs

f2 = codecs.decode("3_1at3a13e13_F0", "rot13")
print(f2)
```

Result:

```text
3_1ng3n13r13_S0
```

The digits and underscores remain unchanged.

**Fragment 3 — Hexadecimal**

The support chat contains a hexadecimal value in the agent logs:

```text
6331346c335f4272317333337d
```

Convert it from hexadecimal to text:

```python
f3 = bytes.fromhex("6331346c335f4272317333337d").decode()
print(f3)
```

Result:

```text
c14l3_Br1s33}
```

### Step 3: Reassemble the fragments

Combine the three decoded fragments in their original order:

```python
import base64
import codecs

f1 = base64.b64decode("Q3liZXJaZXJre0NoNDFu").decode()
f2 = codecs.decode("3_1at3a13e13_F0", "rot13")
f3 = bytes.fromhex("6331346c335f4272317333337d").decode()

print(f1 + f2 + f3)
```

The final result is:

```text
CyberZerk{Ch41n3_1ng3n13r13_S0c14l3_Br1s33}
```

The message means "Broken social engineering chain."

## Flag

`CyberZerk{Ch41n3_1ng3n13r13_S0c14l3_Br1s33}`

## Security Lesson

Social engineering rarely happens in one step. A fake invoice establishes context, a fake technician builds trust, and a fake maintenance agent eventually gains access.

Break the chain by verifying requests through trusted contact details, never installing tools at the request of an unexpected caller, and reporting suspicious maintenance activity to the security team.
