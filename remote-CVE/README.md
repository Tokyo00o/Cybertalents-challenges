# remote-CVE — CTF Writeup

> A web technology identification challenge focused on finding the CVE for an unauthenticated remote code execution vulnerability.

| Challenge | Category | Focus |
| :--- | :--- | :--- |
| **remote-CVE** | Web Security / Vulnerability Research | Wappalyzer, Drupal 7, CVE identification |

## Challenge Description

> What’s CVE ID could be used against the web application in the below target

> Note: its an unauthenticated RCE vulnerability.

**Flag format:** `CVE-xxxx-xxxx`

## Tools & Techniques

- **Wappalyzer** browser extension — identify web technologies
- Web search — research vulnerabilities associated with the detected technology

---

## Walkthrough

### 1. Inspect the Target Web Application

The challenge provided a URL that opened a website with a **login page**.

I began reconnaissance by identifying the application's underlying technology using the **Wappalyzer** browser extension.

Wappalyzer identified:

```text
Drupal 7
```

### 2. Research the RCE Vulnerability

Because the description specifies an **unauthenticated remote code execution (RCE)** issue, I searched for RCE vulnerabilities affecting Drupal 7.

That led to **Drupalgeddon 2**, associated with:

```text
CVE-2018-7600
```

The challenge asks for a CVE ID, not an exploit demonstration, so the investigation ends with the identifier.

> **Note:** The provided target URL and exact technology-detection evidence were not preserved in the original notes.

## Key Takeaways

- Fingerprinting a web application helps narrow down relevant vulnerability research.
- The framework name and major version can be important clues.
- Match the details of a challenge's vulnerability description before selecting a CVE.

## Flag

```text
CVE-2018-7600
```

---

*Writeup based on my original challenge notes.*
