# version — CTF Writeup

> A service-enumeration challenge focused on identifying the web server's version.

| Challenge | Category | Focus |
| :--- | :--- | :--- |
| **version** | Network Enumeration | Nmap, service fingerprinting |

## Challenge Description

> This Challenge will help you understand to define the version of the web service, Can you find the webserver version

**Flag format:** `Xxxxxxy.y.yy`  
**Example:** `Nginx3.4.11`

## Tool

**Nmap** — used for port scanning and service version detection.

---

## Walkthrough

### 1. Scan All Ports and Detect Service Versions

I scanned the target with Nmap:

```bash
nmap <target_ip> -p- -T5 -sV
```

| Option | Purpose |
| :--- | :--- |
| `-p-` | Scan all TCP ports. |
| `-T5` | Use Nmap's fastest/aggressive timing template. |
| `-sV` | Probe open ports to identify services and versions. |

> **Note:** `<target_ip>` is a placeholder; the target address was not recorded in my notes.

### 2. Identify the Web Server

The scan identified a web service on **port 80** with this version information:

```text
Apache httpd 2.4.29 (UNIX)
```

The flag format asks for the server name directly followed by the version, so the answer is `Apache2.4.29`.

## Key Takeaways

- `-sV` helps identify the software and versions behind open network services.
- Version banners can provide the information requested by enumeration challenges.
- Follow the specified flag format rather than copying the full banner.

## Flag

```text
Apache2.4.29
```

---

*Writeup based on my original challenge notes.*
