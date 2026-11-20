# Hook

**Challenge title:** Hameçon  
**Category:** Awareness  
**Difficulty:** Easy  
**Points:** 75  
**Author:** Perceval

## File

- `courriel.eml`

## Solution

### Step 1: Read the headers without opening the link

Inspect the email headers:

```text
From:        "Service RH Tricatel" <rh@tricatel.example>
Return-Path: <no-reply@tricatel-rh-verification.xyz>
Reply-To:    securite-rh@tricatel-rh-verification.xyz
Date:        Wed, 11 Nov 2026 03:12:07 +0000
Subject:     [ACTION REQUISE] Verification de votre compte sous 24h
```

Several details are suspicious:

- The visible `From` address uses `tricatel.example`, but `Return-Path` and `Reply-To` point to `tricatel-rh-verification.xyz`.
- Replies would therefore be directed to a different domain.
- The email was sent at 03:12 UTC, an unusual time for an HR department.
- The message creates pressure with a 24-hour deadline and possible account suspension.
- It uses a generic greeting instead of addressing the recipient by name.

### Step 2: Compare the link text with the real link

Inspect the HTML content of the email:

```html
<a href="http://tricatel-rh-verification.xyz/login?id=Q3liZXJaZXJre24zX2NsMXF1M3pfcGFzXzFuc3AzY3Qzel9sM19sMTNufQ">
https://portail-rh.tricatel.example/securite</a>
```

The visible link text and actual destination are different:

- **Visible text:** `https://portail-rh.tricatel.example/securite`
- **Actual URL:** `http://tricatel-rh-verification.xyz/login?...`

The actual destination uses an external domain and plain HTTP. It appears designed to imitate an internal HR login page and collect credentials.

### Step 3: Decode the ID parameter

The `id` parameter contains Base64 data without its padding.

Add the required padding and decode it:

```python
import base64

s = "Q3liZXJaZXJre24zX2NsMXF1M3pfcGFzXzFuc3AzY3Qzel9sM19sMTNufQ"

print(base64.b64decode(s + "=" * (-len(s) % 4)).decode())
```

The decoded result is:

```text
CyberZerk{n3_cl1qu3z_pas_1nsp3ct3z_l3_l13n}
```

## Flag

`CyberZerk{n3_cl1qu3z_pas_1nsp3ct3z_l3_l13n}`

## Security Lesson

Never click a link before checking its actual destination. Inspect the URL, `Reply-To`, and `Return-Path` headers.

The text displayed in an HTML link can be different from its real destination. Open internal portals by typing their known address directly, and report suspicious emails.
