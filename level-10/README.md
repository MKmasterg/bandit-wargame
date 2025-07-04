## Bandit Level 10 — Extracting Hidden Strings

### Description

In this level, we’re told that the next password is stored in the file `data.txt` and is one of the few **human-readable** strings, **preceded by several `=` characters**.

### Walkthrough

As usual, we start in the home directory when we log in (if not, use `cd ~` to return to it).  
Reminder: for each level, we log in as a different user:  
`bandit{level_number}@bandit.labs.overthewire.org`.

We see a file called `data.txt`. If we try to read it with `cat`, we get a mix of gibberish and occasional readable text — not helpful.

Instead, we can use the `strings` command, which extracts human-readable text from binary or mixed-content files:

```bash
bandit9@bandit:~$ strings data.txt
l0@P(
,k=?
tIsrQ
k`Dl5
...
```

That’s better, but still a lot of noise. Since we know the password is next to a bunch of `=` signs, we can use `grep` to filter those lines:

```bash
bandit9@bandit:~$ strings data.txt | grep -E "={2,}"
========== the
========== password{k
=========== is
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

-   `-E` tells `grep` to use Extended Regular Expressions.
    
-   `={2,}` matches **two or more** `=` characters in a row.
    

#### Note

`strings` is super useful for inspecting binaries or messy files to extract readable text. You can learn more with `man strings`.

From the output, the password is clearly:

```
FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

And that’s the password for the next level!