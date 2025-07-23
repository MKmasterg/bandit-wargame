## Bandit Level 18 — Using diff

### Description

There are 2 files in the homedirectory: `passwords.old` and `passwords.new`. The password for the next level is in `passwords.new` and is the only line that has been changed between `passwords.old` and `passwords.new`.

### Walkthrough

Let’s use `diff` to see the diffrences between the two files:

```bash
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< C6XNBdYOkgt5ARXESMKWWOUwBeaIQZ0Y
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```
- `42c42`: This means that line 42 in the original file has been changed (c) in the new file.

- `< C6XNBdYOkgt5ARXESMKWWOUwBeaIQZ0Y`: This is the line 42 in the first file.

- `> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`: This is the line 42 in the second file.

Since we've been told that the password is in `passwords.new`, that means our password for the next level is:

```bash
x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```