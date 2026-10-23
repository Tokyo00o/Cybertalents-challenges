# hidden — CTF Writeup

> A web content-discovery challenge: identify a hidden directory using a wordlist.

| Challenge | Category | Focus |
| :--- | :--- | :--- |
| **hidden** | Web Enumeration | ffuf, directory fuzzing, wordlists |

## Challenge Description

> Can you find the hidden directory on the target web app?

**Flag format:** Submit **only the directory name**.

## Tool

**ffuf** — a web fuzzer used here to discover directories by trying candidate path names.

---

## Walkthrough

### 1. Fuzz the Target's Directory Paths

I used **ffuf** with a common directory wordlist:

```bash
ffuf -u 'http://<ip-or-domain>/FUZZ' -w /usr/share/wordlists/dirb/common.txt
```

| Component | Purpose |
| :--- | :--- |
| `-u` | Specify the target URL. |
| `FUZZ` | Mark the path segment where ffuf should insert wordlist entries. |
| `-w` | Supply the list of candidate directory names. |

> **Note:** Replace `<ip-or-domain>` with the challenge target; the exact target was not saved in the original notes.

### 2. Identify the Hidden Directory

The fuzzing results included multiple directories. The one relevant to the challenge was:

```text
secret
```

Since the challenge requires only the directory's name, no URL prefix or slash is needed.

## Key Takeaways

- Wordlist-based fuzzing can reveal paths that are not linked from a site's main pages.
- `FUZZ` determines which part of the requested URL is varied.
- Carefully follow the expected flag format.

## Flag

```text
secret
```

---

*Writeup based on my original challenge notes.*
