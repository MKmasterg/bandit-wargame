## Bandit Level 2 — Reading Files (again)

### Description

In this level, we’re told the next password is in the `-` file in the home directory.

### Walkthrough

Since we’re already in the home directory (as we landed here after logging in from level 0; if you're not you can use the command `cd ~` to go to the home directory), we can list the files in the directory using:

```bash
ls
```

We should see the `-` file listed; However, we can’t just use a normal cat command here because the filename starts with a dash, which is typically interpreted as a command-line option.

To avoid that, we can tell the shell explicitly that we’re referring to a file in the current directory by using:
```bash
cat ./-
```

This will output the password for the next level:

```
263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

And there it is — we’ve got the password!