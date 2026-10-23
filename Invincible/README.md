# Invincible — CTF Writeup

> An OSINT challenge involving username research and recovery of an older version of a public profile.

| Challenge | Category | Focus |
| :--- | :--- | :--- |
| **Invincible** | OSINT | Username searches, Disqus, Wayback Machine |

## Challenge Description

> This Challenge will help you understand the basics of OSINT
>
> Our threat intelligence team has noticed suspicious behavior from the user "soremanzo" , Can you trace him and find what he is trying to hide.

## Tools & Resources

- [OSINT Framework](https://osintframework.com/) — directory of OSINT resources
- [NameCheckup](https://namecheckup.com/) — username search/checking tool
- [Internet Archive / Wayback Machine](https://web.archive.org/) — archived versions of web pages

---

## Walkthrough

### 1. Search for the Username

The key clue in the description is the username:

```text
soremanzo
```

I used the [OSINT Framework](https://osintframework.com/) to explore username search tools, following the path:

```text
Username → Username search engines
```

I then checked `soremanzo` using [NameCheckup](https://namecheckup.com/). One of the services associated with the username was **Disqus**.

### 2. Inspect the Disqus Profile

I visited the user's Disqus profile:

[disqus.com/by/soremanzo/](https://disqus.com/by/soremanzo/)

The profile's **About** section included a message that the flag had been deleted and hinted that not everything could be found by looking only at the current page:

> Not all what you need will be on your way back

This suggested checking an earlier version of the page.

### 3. Look Back with the Wayback Machine

I searched for the Disqus profile in the [Wayback Machine](https://web.archive.org/web/*/https://disqus.com/by/soremanzo/).

According to my notes, the archived snapshot dated **May 5, 2021** contained the flag that was no longer visible on the current profile.

## Key Takeaways

- Reusing a username across platforms can connect a person's public profiles.
- Profile descriptions may contain direct or indirect clues.
- Archived pages can preserve information that has since been deleted or edited.

## Flag

```text
FLAG{4rch1v3_Usu4LLy_C0nt41n_Us3fuL_1nf0rm4t10n}
```

---

*Writeup based on my original challenge notes.*
