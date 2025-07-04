## Bandit Level 3 — Reading Files (again)

### Description

In this level, we’re told the next password is in the `spaces in this filename` file in the home directory.

### Walkthrough

Since we’re already in the home directory (as we landed here after logging in from the previous level; if you're not you can use the command `cd ~` to go to the home directory. please note that for each level we should log into another user of the host domain: bandit{level_number}@bandit.labs.overthewire.org), we can list the files in the directory using:

```bash
ls
```

We should see the `spaces in this filename` file listed; However, we can’t just use a normal cat command here because the filename contains spaces.
we have to escape the spaces or use quotes:
```bash
cat ./spaces\ in\ this\ filename
```
or
```bash
cat "spaces in this filename"
```

This will output the password for the next level:

```
MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```