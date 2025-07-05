## Bandit Level 11 — Decoding Base64

### Description

In this level, we’re told that the next password is stored in the file `data.txt`, and that the file contains **base64 encoded data**.

### Walkthrough

As usual, we start in the home directory when we log in (if not, use `cd ~` to return to it).
Reminder: for each level, we log in as a different user:
`bandit{level_number}@bandit.labs.overthewire.org`.

There’s a file called `data.txt`. If we read it with `cat`, we see a long string ending in multiple `=` signs — a common pattern in Base64 encoding:

```bash
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIGR0UjE3M2ZaS2IwUlJzREZTR3NnMlJXbnBOVmozcVJyCg==
```

To decode it, we can pipe the contents through the `base64` command with the `-d` flag (for decode):

```bash
bandit10@bandit:~$ cat data.txt | base64 -d
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

We’ve got the password!

#### Note

`base64` is a command-line utility for encoding and decoding data using the Base64 format. It’s often used to safely encode binary data as readable ASCII text.
Base64 strings often end in one or two `=` signs, which are used as padding.

And that’s the password for the next level!