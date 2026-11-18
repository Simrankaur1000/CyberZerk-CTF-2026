# Calls for Help

**Challenge title:** Appels à l'aide  
**Category:** Forensics  
**Difficulty:** Intro  
**Points:** 100  
**Author:** Perceval

## File

- `appels_a_l_aide.pcap` — the network capture provided with the challenge.

## Solution

### 1. Open the capture

Open `appels_a_l_aide.pcap` in Wireshark using **File → Open**.

### 2. Show HTTP traffic

Click the filter bar at the top, type `http`, and press **Enter**.

### 3. Find the message

Look for the packet containing:

`POST /adieu.php HTTP/1.1`

Right-click that packet and select **Follow → TCP Stream**. Wireshark will show the full request and the data sent with it.

### 4. Read the message

The request contains these details:

- **Employee:** `Marc_V`
- **Status:** `epuisement_total`
- **Message:** “Je quitte Tricatel. Le management me détruit la santé mentale. Je n'ai plus dormi depuis 3 jours. Adieu.”

The message was sent in an HTTP POST request to `/adieu.php`. In this capture, the connection is between `127.0.0.1` and port `9999`.

The flag is included at the end of the submitted message.

## Flag

```text
CyberZerk{Exf1ltr4t10n_HTTP_D3s3sp01r}
```
