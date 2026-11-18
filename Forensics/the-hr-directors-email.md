# The HR Director's Email

**Category:** Forensics  
**Difficulty:** Easy  
**Points:** 75  
**Author:** Perceval

## Files

- `note_du_drh.eml` — original email file.
- `note_interne_rh.txt` — attachment extracted from the email.

## Solution

### 1. Inspect the email headers

Open the `.eml` file in a text editor or inspect its raw source. The sender information is suspicious:

```text
Return-Path: <attaquant@mail-cheap-relay.ru>
Received: from vps-unknown.mail-cheap-relay.ru
  (vps-unknown.mail-cheap-relay.ru [45.83.12.207])
From: "Direction des Ressources Humaines" <drh@tricatel-cyber.fr>
Reply-To: <attaquant@mail-cheap-relay.ru>
```

Although the visible `From` address claims to be the HR department, the return path and reply-to address point to an unrelated external domain.

### 2. Check email authentication

The authentication results show:

- **SPF:** fail — the sending IP `45.83.12.207` is not authorized to send mail for the claimed domain.
- **DKIM:** none — no DKIM signature was present.
- **DMARC:** fail — authentication failed for the visible `tricatel-cyber.fr` domain.

These results indicate that the displayed sender was not authenticated.

### 3. Extract the attachment

The email contains a Base64-encoded attachment named `note_interne_rh.txt`. Extract the attachment from the MIME message and decode it.

For example, use Python's email parser to save the attachment:

```python
from email import policy
from email.parser import BytesParser

with open("note_du_drh.eml", "rb") as f:
    message = BytesParser(policy=policy.default).parse(f)

for part in message.iter_attachments():
    filename = part.get_filename()
    if filename == "note_interne_rh.txt":
        with open(filename, "wb") as output:
            output.write(part.get_payload(decode=True))
        print(f"Extracted: {filename}")
```

### 4. Read the decoded attachment

The extracted text contains an internal HR memo about monitoring sick leave and includes the proof string:

```text
Preuve de l'ordre : CyberZerk{M4il_Sp00f3_P0ur_Surv31ll3r_L3s_Arr3ts}
```

## Flag

```text
CyberZerk{M4il_Sp00f3_P0ur_Surv31ll3r_L3s_Arr3ts}
```
