# Sick Leave

**Challenge title:** Arrêt Maladie\
**Category:** Web\
**Difficulty:** Intro\
**Points:** 100\
**Author:** Perceval

## File

- Application: https://absences.cyberzerk.fr

## Solution

### Step 1: Inspect your own absence record

Open the absence portal and access your own absence request.

The authenticated session showed:

- Employee: `#8472`
- Access level: Standard rights
- Your absence record ID: `1042`

The record ID was supplied through the `dossier_id` URL parameter:

```text
https://absences.cyberzerk.fr/?dossier_id=1042
```

### Step 2: Change the record ID

The challenge description stated that the colleague's sick-leave request was submitted just before yours. Since the record IDs were sequential, change the `dossier_id` parameter from `1042` to `1041`:

```text
https://absences.cyberzerk.fr/?dossier_id=1041
```

### Step 3: Read the colleague's report

The portal displayed the previous record:

- File: `#1041 - Extended sick leave`
- Employee: Marc V. (Senior Developer)
- Occupational Health Report: Severe burnout, 70-hour workweeks, total isolation, and moral harassment from management.
- Recommended HR action: Contest sick leave and block the employee's IT access.

### Step 4: Obtain the flag

The flag was displayed in the colleague's absence report.

## Flag

```text
CyberZerk{ID0R_M4n4g3m3nt_T0x1qu3}
```

## Security Lesson

This challenge demonstrates Insecure Direct Object Reference (IDOR), a form of Broken Access Control. The application allowed an authenticated user to change the `dossier_id` parameter and access another employee's confidential medical record without proper authorization.

Applications must verify server-side that the authenticated user is authorized to access the specific record requested. Unpredictable identifiers alone are not an authorization control.
