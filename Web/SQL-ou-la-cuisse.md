# SQL or Thigh?

**Challenge title:** SQL or Thigh?  
**Category:** Web  
**Difficulty:** Intro  
**Points:** 100  
**Author:** Perceval

## File

- Application: https://sql.cyberzerk.fr

## Solution

### Step 1: Inspect the login form

Open the challenge website and locate the username and password fields.

### Step 2: Inject SQL into the username field

Enter the following SQL injection payload as the username:

```sql
' OR 1=1-- -
```

Enter the following as the password:

```text
1234
```

### Step 3: Bypass authentication

Submit the login form.

The application accepted the input and authenticated the session, displaying:

```text
HR MANAGEMENT AREA: AUTHENTICATED SESSION

Connection successful.
```

The injected condition `OR 1=1` makes the SQL condition true, while the comment sequence `-- -` causes the remainder of the SQL statement to be ignored in the relevant SQL syntax.

### Step 4: Retrieve the confidential memo

After logging in, the HR area displayed the internal file `Memo_Direction.txt`:

> The dev team is at its limit. We don't replace anyone, just give them a foosball table and remind them that we are a family to compensate for the 60 hours a week. If they crack, it's because they're not made for cyber.

### Step 5: Obtain the flag

The flag was displayed in the authenticated HR area.

## Flag

```text
CyberZerk{SQL_1nj3ct10n_C4us3d_By_Burn0ut}
```

## Security Lesson

This challenge demonstrates SQL Injection leading to authentication bypass. The login system appears to incorporate user-controlled input into an SQL query without safely parameterizing it.

Applications should use prepared statements or parameterized queries, validate inputs appropriately, and avoid constructing SQL statements through string concatenation. Authentication must not depend on user input being interpreted as part of a trusted SQL expression.
