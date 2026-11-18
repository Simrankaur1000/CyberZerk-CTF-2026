# The HR Director's Email

**Challenge title:** Mail du DRH  
**Category:** Forensics  
**Difficulty:** Easy  
**Points:** 75  
**Author:** Perceval

## Files

- `note_du_drh.eml` — the email provided in the challenge.
- `note_interne_rh.txt` — the attachment inside the email.

## Solution

### 1. Open the email

Open `note_du_drh.eml` with an email application such as Thunderbird.

Check the sender details. The email claims to be from the HR department, but the reply address belongs to a different domain:

- **From:** `drh@tricatel-cyber.fr`
- **Reply-To:** `attaquant@mail-cheap-relay.ru`

This is a sign that the sender address may have been forged.

### 2. Save the attachment

The email contains an attachment named `note_interne_rh.txt`.

Save the attachment to your computer, then open it with Notepad.

### 3. Find the flag

Read the text file. The flag is written near the bottom, after the line `Preuve de l'ordre:`.

## Flag

```text
CyberZerk{M4il_Sp00f3_P0ur_Surv31ll3r_L3s_Arr3ts}
```
