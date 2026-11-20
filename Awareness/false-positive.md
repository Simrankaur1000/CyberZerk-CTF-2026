# False Positive

**Challenge title:** Faux Positif  
**Category:** Awareness  
**Difficulty:** Easy  
**Points:** 62  
**Author:** Perceval

## File

- `boite_reception.txt`

## Solution

### Step 1: Compare the authentication headers

Each of the four emails contains an `Authentication-Results` header.

Comparing the sender, relay server, and authentication results identifies the suspicious email:

| Mail | From | Received from | SPF / DKIM / DMARC |
|---|---|---|---|
| 1 | `paie@tricatel-cyber.fr` | `mx.tricatel-cyber.fr (192.168.10.4)` | pass / pass / pass |
| 2 | `it-support@tricatel-cyber.fr` | `mx.tricatel-cyber.fr (192.168.10.4)` | pass / pass / pass |
| 3 | `"DRH Tricatel" <rh@tricatel-cyber.fr>` | `srv-ovh-237.cheap-bulk-mail.ru (45.83.12.207)` | fail / none / fail |
| 4 | `newsletter@tricatel-cyber.fr` | `mx.tricatel-cyber.fr (192.168.10.4)` | pass / pass / pass |

Mail 3 stands out because its authentication checks fail and it was relayed through an external bulk-mail server.

### Step 2: Confirm with the other indicators

Mail 3 contains several phishing indicators:

- It claims to come from `tricatel-cyber.fr`, but was relayed by an external `.ru` bulk-mail server.
- SPF and DMARC fail, and no DKIM signature is present.
- It uses urgency and threats, including "URGENT", "suspended", and "within 24h".
- Its link points to `tricatel-cyber.secure-login[.]ru`, a lookalike domain.

These indicators identify Mail 3 as the phishing email.

### Step 3: Decode the tracking ID

The challenge hint suggests that the tracking ID contains useful information.

Decode all four `X-Tracking-Id` values from Base64:

```python
import base64

tracking_ids = [
    "bm90X3BoaXNoaW5nXzE=",
    "bm90X3BoaXNoaW5nXzI=",
    "Q3liZXJaZXJre1VuX0Iwbl9SM2ZsM3gzX1VuX0NsMWNfRXYxdDN9",
    "bm90X3BoaXNoaW5nXzQ="
]

for tracking_id in tracking_ids:
    print(base64.b64decode(tracking_id).decode())
```

The output is:

```text
not_phishing_1
not_phishing_2
CyberZerk{Un_B0n_R3fl3x3_Un_Cl1c_Ev1t3}
not_phishing_4
```

The three legitimate emails contain `not_phishing` identifiers, while Mail 3 contains the flag.

## Flag

`CyberZerk{Un_B0n_R3fl3x3_Un_Cl1c_Ev1t3}`

## Security Lesson

The display name and `From` address can both be forged. Check the `Received` chain, SPF/DKIM/DMARC results, and the actual link domain.

Urgency and threats are common phishing indicators. Report suspicious emails instead of clicking their links.
