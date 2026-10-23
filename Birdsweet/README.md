# Birdsweet — CTF Writeup

> A web challenge that combines HTTP header modification, parameter discovery, and decoding.

| Challenge | Category | Focus |
| :--- | :--- | :--- |
| **Birdsweet** | Web Security | User-Agent, Burp Suite, parameter fuzzing, encoding |

## Challenge Description

> I am tired of using firefox, so I made birdsweet!

## Tools & Techniques

- **Burp Suite Repeater** — edit and resend requests
- **Burp Suite Intruder** — test values of a request parameter
- **RapidTables** — [Hex to ASCII converter](https://www.rapidtables.com/convert/number/hex-to-ascii.html)
- **CyberChef** — [decode encoded output](https://gchq.github.io/CyberChef/)

---

## Walkthrough

### 1. Bypass the Browser Check

The supplied challenge URL displayed this message:

```text
This website only works if you have the birdsweet browser (or an IQ of 130+)
```

There is no conventional browser called *birdsweet*. However, servers can check which browser a client claims to be using through the HTTP `User-Agent` header.

I intercepted the request in Burp Suite, sent it to **Repeater**, and changed the header to:

```http
User-Agent: birdsweet
```

The server then responded with a new hint:

```text
If I remember correctly this button will take you to the flag, but i think it is broken
```

### 2. Inspect the Button and Its Parameter

Inspecting the page's source showed that the button sends a request containing an `id` query parameter.

I tried:

```http
GET /?id=1 HTTP/1.1
```

The response was:

```text
no flag here
```

Changing the value manually did not immediately find the correct one, so I moved on to a parameter brute-force attempt.

### 3. Fuzz the `id` Parameter

I sent the request to **Burp Suite Intruder**, highlighted the `id` value as the payload position, and configured a numeric payload range:

| Setting | Value |
| :--- | :--- |
| Payload type | Numbers |
| From | `0` |
| To | `100` |
| Target | `id` query parameter |

After starting the attack, I compared **response lengths** to identify an outlier. The response for `id=26` had a different length.

```http
GET /?id=26 HTTP/1.1
```

### 4. Decode the Encoded Response

The response contained the flag data in **hexadecimal** form. I converted the hex data to text using a [Hex to ASCII converter](https://www.rapidtables.com/convert/number/hex-to-ascii.html).

The resulting text was **Base64-encoded**, so I used [CyberChef](https://gchq.github.io/CyberChef/) to decode it.

> **Note:** The original notes do not include the raw hexadecimal response or the intermediate Base64 text, so those artifacts are not reconstructed here.

---

## Key Takeaways

- An application's browser check may depend only on a client-controlled HTTP header.
- HTML and JavaScript inspection can expose hidden request parameters.
- Response-length differences can help identify interesting results during parameter fuzzing.
- Hexadecimal and Base64 are encodings, not encryption.

## Flag Recorded in My Notes

```text
Flag{QCFATjNDUFg5ZHhIK0ZHU0d3WHhYSFNiNE5jU0FNVUJDWDIxWkFkOWtjMkZhVlJNZlpnMGhuQjJSNWtJaXpFamcyeUkyNzRZTjZHTnJrRlEvNktOSHpWMFE9PTFjMmQ5MmZkODMzZmU2MGI=}
```

---

*Writeup based on my original challenge notes; the flag string is preserved exactly as recorded.*
