# Open Doors — CTF Writeup

> An introductory port-scanning challenge where the number of open ports must be submitted as an MD5 hash.

| Challenge | Category | Focus |
| :--- | :--- | :--- |
| **Open Doors** | Network Enumeration | Nmap, open ports, MD5 |

## Challenge Description

> This Challenge will help you understand basic port scanning
>
> How Many ports under 100 are opened in the targets

**Flag format:** `md5(ports)`

## Tools & Techniques

- **Nmap** — scan the specified port range
- **MD5** — hash the number of open ports to obtain the flag

---

## Walkthrough

### 1. Scan the Target's Low-Numbered Ports

I used Nmap to scan the target:

```bash
nmap <ip> -p0-100 -T5
```

| Option | Purpose |
| :--- | :--- |
| `-p0-100` | Scan ports from 0 through 100. |
| `-T5` | Use the fastest/aggressive timing template. |

> **Note:** The target IP was not included in the original notes.

### 2. Count the Open Ports

The scan found **two open ports**:

```text
22
80
```

Therefore, the value to hash is:

```text
2
```

### 3. Calculate the MD5 Hash

For example, the hash can be generated with:

```bash
printf '2' | md5sum
```

Using `printf` avoids adding a newline to the hashed input.

The MD5 hash of `2` is:

```text
c81e728d9d4c2f636f067f89cc14862c
```

## Key Takeaways

- Nmap identifies open ports on a target.
- Check the requested port range and count only ports reported as open.
- Hash the exact intended string; an extra newline changes an MD5 result.

## Flag

```text
c81e728d9d4c2f636f067f89cc14862c
```

---

*Writeup based on my original challenge notes; the sample MD5 command is provided to reproduce the recorded result.*
