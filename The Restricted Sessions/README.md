# The Restricted Sessions — CTF Writeup

> A web security challenge involving session identifiers, exposed session data, and a user-information endpoint.

| Challenge | Category | Focus |
| :--- | :--- | :--- |
| **The Restricted Sessions** | Web Security | Session handling, HTTP requests, authorization |

## Challenge Description

> Flag is restricted to logged users only , can you be one of them.

The objective is to reach a page that reveals its flag only to an authenticated user.

## Tools & Techniques

- Browser Developer Tools for inspecting the page and its scripts
- HTTP request inspection and modification
- Examination of exposed application paths and session parameters

---

## Walkthrough

### 1. Inspect the Login-Restricted Page

Opening the challenge URL shows that the flag is available only to logged-in users.

On inspecting the page, I noticed JavaScript communicating with an endpoint named:

```text
getcurrentuserinfo.php
```

The script uses a session identifier associated with the `PHPSESSID` cookie to retrieve user information.

### 2. Investigate the Session Lookup

Visiting `getcurrentuserinfo.php` directly did not provide useful information.

I returned to the main page and tried supplying a `PHPSESSID` value. The application responded with an error revealing a server-side session data path:

```text
Session not found in data/session_store.txt
```

I then inspected the exposed path:

```text
/data/session_store.txt
```

It displayed session identifiers belonging to other users.

### 3. Test the Additional User Parameter

Trying one of those session identifiers indicated that another parameter, `UserInfo`, was also required. Adding a random `UserInfo` value produced:

```text
Validation failed
```

Trying other session identifiers at that point did not resolve the issue.

### 4. Request User Information with a Known Session ID

After reviewing the page's script again, I sent a **POST** request to `getcurrentuserinfo.php`, placing one of the session identifiers found in `session_store.txt` in the request body under the `PHPSESSID` parameter.

This time, the endpoint returned a **username**. Returning to the main page with the recovered session-related information allowed me to retrieve the flag.

> **Note:** My original notes do not include a sample session ID, the exact final request, or the returned username, so those values are intentionally omitted.

---

## Key Takeaways

- Error messages can disclose sensitive internal file paths.
- Exposed session information can undermine access controls.
- Inspecting client-side scripts helps explain how an application handles session lookups.
- Testing request methods and parameter placement can reveal differences in endpoint behavior.

## Flag

```text
sessionareawesomebutifitsecure
```

---

*Writeup based on my original challenge notes.*
