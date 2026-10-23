# s3cret — CTF Writeup

> A Linux fundamentals question about deleting a file from the current user's home directory.

| Challenge | Category | Focus |
| :--- | :--- | :--- |
| **s3cret** | Linux Fundamentals | `rm`, home directory shortcut |

## Challenge Description

> I need to remove a file called s3cret in my home directory. which command should i use

## Solution

Use the `rm` command with a path referring to the file in the current user's home directory:

```bash
rm ~/s3cret
```

### Command Breakdown

| Part | Meaning |
| :--- | :--- |
| `rm` | Removes a file. |
| `~` | Expands to the current user's home directory, such as `/home/<username>`. |
| `/s3cret` | Names the file to remove. |

The full path represented by `~/s3cret` is the `s3cret` file inside the user's home directory.

> **Caution:** `rm` removes files; check the path before running the command.

## Key Takeaways

- `~` is a convenient way to refer to your home directory.
- `rm` deletes a file when you provide its path.

## Answer

```bash
rm ~/s3cret
```

---

*Writeup based on my original challenge notes.*
