# Discreet Tunnel

**Challenge title:** Tunnel Discret  
 
**Category:** Forensics  
**Points:** 175  
**Difficulty:** Hard  
**Author:** Perceval

## Challenge

A computer is sending many DNS requests. The challenge asks what information the process is sending out.

The capture contains data hidden inside DNS query names. This technique is called **DNS tunneling** or **DNS exfiltration**.

## File

- `capture.pcap` — network capture

## Solution

### 1. Open the capture

Open `capture.pcap` in Wireshark.

To show DNS traffic only, enter this display filter:

```
dns
```

The capture contains 14 packets.

### 2. Identify the suspicious DNS requests

The suspicious requests are packets 0–9. They are sent from:

- Source: `10.10.0.57`
- DNS server: `10.10.0.1`

The remaining four requests (packets 10–13) are normal lookups for company services such as the intranet, mail, wiki, and proxy.

The suspicious query names have a pattern like:

```
00.43796265725a.exfil-4167c682.tricatel-interne.fr
01.65726b7b646e.exfil-4167c682.tricatel-interne.fr
```

The first two digits are sequence numbers. The hexadecimal text after them contains pieces of the hidden message.

### 3. Put the pieces in order

Use the sequence numbers from `00` to `09`. The hexadecimal pieces are:

| Sequence | Hexadecimal | Decoded text |
| --- | --- | --- |
| 00 | `43796265725a` | `CyberZ` |
| 01 | `65726b7b646e` | `erk{dn` |
| 02 | `735f33786631` | `s_3xf1` |
| 03 | `6c7472347431` | `ltr4t1` |
| 04 | `306e5f723334` | `0n_r34` |
| 05 | `7373336d626c` | `ss3mbl` |
| 06 | `335f6c33735f` | `3_l3s_` |
| 07 | `733075735f64` | `s0us_d` |
| 08 | `306d34316e33` | `0m41n3` |
| 09 | `737d` | `s}` |

### 4. Decode and join the pieces

Each hexadecimal pair represents one text character. For example:

```
43 79 62 65 72 5a
```

converts to:

```
CyberZ
```

Converting each piece and joining them in sequence gives the complete hidden message.

### 5. Flag

```
CyberZerk{dns_3xf1ltr4t10n_r34ss3mbl3_l3s_s0us_d0m41n3s}
```

## What I learned

DNS is normally used to look up domain names. In DNS tunneling, data can be placed inside DNS query names and sent to another system. Sequence numbers help the receiver put the pieces back together.
