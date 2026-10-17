# Linuxcmd 101 — CTF Writeup

> A beginner-friendly Linux challenge that combines command-line navigation, archive extraction, file inspection, password discovery, and basic decoding.

| Challenge | Category | Focus |
| :--- | :--- | :--- |
| **Linuxcmd 101** | Forensics / Linux Fundamentals | Linux CLI, ZIP archives, ELF files, Base64, ROT13 |

**Challenge file:** [Download `linux_chal.tar.gz`](https://dr-hubchallenges.s3.us-west-2.amazonaws.com/Forensics/linux_chal.tar.gz)

## Challenge Description

> This Challenge will help you understand essential commands in Linux OS, Each point is linked to another point, connect the link and win the Flag!

The goal is to follow a chain of clues hidden inside files and password-protected archives until the final flag is recovered.

## Tools & Commands

- **Linux utilities:** `ls`, `cd`, `cat`, `file`, `gzip`, `tar`, and `unzip`
- **Password recovery:** `zip2john` and `john` (John the Ripper)
- **Decoding:** Base64 and ROT13

---

## Walkthrough

### 1. Extract the Initial Archive

The challenge provides a compressed file named `linux_chal.tar.gz`. Extract the gzip layer first, then unpack the tar archive:

```bash
gzip -d linux_chal.tar.gz
tar -xf linux_chal.tar
```

This reveals a directory named `cat`. Move into it and inspect its contents, including hidden files:

```bash
cd cat
ls -la
```

Two interesting files appear:

```text
.pass.txt
exec.zip
```

### 2. Find the Password for `exec.zip`

The hidden file `.pass.txt` contains a password:

```bash
cat .pass.txt
```

```text
2434237800
```

Use it when prompted to extract the archive:

```bash
unzip exec.zip
```

The extracted directory is named `exec`.

### 3. Investigate the Unusual `-` File

Enter the new directory:

```bash
cd exec
ls -la
```

Inside are `ascii.zip` and a file literally named `-`.

Because `-` has special meaning for many command-line tools (often representing standard input), reference the file with an explicit relative path:

```bash
file ./-
```

The `file` utility identifies it as an **ELF executable**. Run it:

```bash
./-
```

The program prints:

```text
Here is your password: 'passforasciiii'
```

That gives us the password for the next archive.

### 4. Extract `ascii.zip` and Inspect the Files

Extract `ascii.zip` using the password from the executable:

```bash
unzip ascii.zip
```

The extraction produces `size37.zip` and a series of files named `f0` through `f8`.

Rather than opening every file immediately, check their types:

```bash
file f*
```

The file `f6` stands out as text. Read it:

```bash
cat f6
```

```text
thisisasciiiiiprintapleeeee
```

This is the password for `size37.zip`:

```bash
unzip size37.zip
```

### 5. Inspect the `test` Files and Continue

The next extraction produces `next.zip` and several text files named `test1` through `test7`.

First, inspect their types:

```bash
file test*
```

Since all the `test` files are text, read them together instead of opening them one by one:

```bash
cat test*
```

Among the output is the familiar string:

```text
thisisasciiiiiprintapleeeee
```

Reuse it as the password for `next.zip`:

```bash
unzip next.zip
```

### 6. Analyze the Wordlist for `NumberOne.zip`

This archive reveals two items:

```text
NumberOne.zip
nexttocybertalents
```

Identify and read the second file:

```bash
file *
cat nexttocybertalents
```

It looks like a wordlist, so I initially tried using **John the Ripper** against `NumberOne.zip`:

```bash
zip2john NumberOne.zip > lock
john lock --wordlist=nexttocybertalents
```

That attempt did **not** recover the password.

After reviewing the wordlist manually, I noticed an unusual pattern involving longer entries and the ending `orderby1337`. I tested the following candidate:

```text
orderby1337
```

It worked as the password for `NumberOne.zip`.

### 7. Recover the Next ZIP Password

The contents of `NumberOne.zip` led to another password-protected ZIP archive. This time, I recovered the password through brute force:

```text
rocky
```

After extracting that archive, I found a file named `pass` alongside `decodeme.zip`.

> **Note:** My original notes record the recovered password but not the exact brute-force command, so I have not invented one here.

### 8. Decode the Password for `decodeme.zip`

Read the `pass` file:

```bash
cat pass
```

```text
dXNlbWVhc3Bhc3N3b3Jk
```

Using this string directly as the ZIP password fails. The archive name, `decodeme.zip`, suggests that the string needs to be decoded first.

It is **Base64-encoded**. You can decode it in the terminal:

```bash
echo 'dXNlbWVhc3Bhc3N3b3Jk' | base64 -d
```

Output:

```text
usemeaspassword
```

Now extract `decodeme.zip` and enter the decoded password when prompted:

```bash
unzip decodeme.zip
```

### 9. Decode the Final Flag with ROT13

The extracted content reveals what looks like a flag:

```text
synt{f1zcyr_yvahk_101}
```

However, submitting this value does not work. The text is encoded with **ROT13**, a substitution cipher that shifts letters by 13 positions.

You can decode it using a terminal command:

```bash
echo 'synt{f1zcyr_yvahk_101}' | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

Or identify the encoding with [dCode's Cipher Identifier](https://www.dcode.fr/cipher-identifier).

The decoded flag is:

```text
flag{s1mple_linux_101}
```

---

## Password Trail

| Archive | Where the password came from | Password |
| :--- | :--- | :--- |
| `exec.zip` | Hidden file `.pass.txt` | `2434237800` |
| `ascii.zip` | Output of the ELF executable `./-` | `passforasciiii` |
| `size37.zip` | Text file `f6` | `thisisasciiiiiprintapleeeee` |
| `next.zip` | Contents of the `test*` files | `thisisasciiiiiprintapleeeee` |
| `NumberOne.zip` | Manual inspection of `nexttocybertalents` | `orderby1337` |
| Additional ZIP archive | Brute-force recovery | `rocky` |
| `decodeme.zip` | Base64-decoded contents of `pass` | `usemeaspassword` |

## Key Takeaways

- Use `ls -la` to reveal hidden files that a normal `ls` may miss.
- Use `file` to identify unknown files before opening or executing them.
- Prefix unusual filenames such as `-` with `./` so commands treat them as file paths.
- Use shell wildcards (`*`) to inspect multiple related files efficiently.
- When a wordlist attack fails, re-examine the clues instead of assuming the approach is correct.
- Recognize that **encoding is not encryption**: Base64 and ROT13 can be reversed without a secret key.

## Flag

```text
flag{s1mple_linux_101}
```

---

*Writeup based on my solution notes for the Linuxcmd 101 challenge.*
