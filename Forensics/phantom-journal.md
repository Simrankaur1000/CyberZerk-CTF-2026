# Phantom Journal

**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 125  
**Author:** Perceval

## File

- `pointeuse.sqlite` — the SQLite database containing attendance records.

## Solution

### 1. Understand the challenge

The database contains attendance records. One night-time record was deleted, and we need to recover its information.

### 2. Open the database

Use **DB Browser for SQLite**:

1. Open DB Browser for SQLite.
2. Click **Open Database**.
3. Select `pointeuse.sqlite`.
4. Open the **Browse Data** tab.
5. Select the `pointages` table.

### 3. Find the missing record

The visible records have IDs 1, 2, 3 and 5. Record ID **4** is missing.

This suggests that a record may have been deleted.

### 4. Recover the deleted information

Deleting a row in SQLite does not always erase its data immediately. Some information can remain in unused space inside the database file.

The raw contents of the database were examined, and this text was recovered:

```text
ADMIN_SYS CyberZerk{sql1t3_d3l3t3_n3_v1d3_pas_l3s_p4g3s} 2026-11-03 02:03
```

The recovered record is:

| Field | Value |
|---|---|
| ID | 4 (deleted) |
| Agent | ADMIN_SYS |
| Badge | Flag |
| Date and time | 2026-11-03 02:03 |

This identifies the missing entry as a night-time attendance record for `ADMIN_SYS`.

## Flag

```text
CyberZerk{sql1t3_d3l3t3_n3_v1d3_pas_l3s_p4g3s}
```

**Note:** DB Browser for SQLite can show the remaining records, but its normal Browse Data view does not display deleted rows. Recovering the old text requires examining the raw database file.
