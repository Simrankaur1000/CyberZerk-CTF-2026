# Crisis Dump

**Challenge title:** Dump de Crise  

**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 150  
**Author:** Perceval

## File

- `depot_intranet.tgz` — an archive containing a Git repository.

## Solution

### 1. Understand the challenge

The developer says that the secret was removed from the code.

However, Git keeps previous versions of files in its history. This means a deleted secret may still be recoverable from an older commit.

### 2. Extract the archive

1. Locate `depot_intranet.tgz`.
2. Right-click the file and extract it using 7-Zip or another archive tool.
3. Open the extracted `depot` folder.

You should find the project files and a hidden `.git` folder.

The `.git` folder contains the repository's commit history. Do not delete it.

### 3. Open Command Prompt in the project folder

1. Open the extracted `depot` folder in File Explorer.
2. Click the address bar.
3. Type `cmd` and press Enter.

Command Prompt will open in that folder.

### 4. Check the Git history

Run:

```bash
git log --oneline
```

This displays the previous commits.

Look for:

```text
b3fc794 Securisation: cle sortie du code (lue depuis env)
```

The commit message indicates that the developer removed the key from the code.

### 5. Check what was removed

Run:

```bash
git show b3fc794
```

Look for lines beginning with a minus sign (`-`). These show the code removed in that commit.

The important removed line is:

```text
API_KEY="CyberZerk{G1t_N0ubl13_J4m41s_R13n_Du_P4ss3}"
```

The key was removed from the latest version, but it is still present in the earlier Git history.

### 6. Find the flag

The removed API key contains the flag:

```text
CyberZerk{G1t_N0ubl13_J4m41s_R13n_Du_P4ss3}
```

## Real-world lesson

Removing a password or API key from the latest code does not remove it from previous Git commits.

If a real secret is exposed, it should be revoked and replaced. The Git history should also be cleaned when necessary.
