## Bandit Level 1 — Reading Files

### Description

In this level, we’re told to find the `readme` file in the home directory of the current user and read it to get the password for the next level.

### Walkthrough

Since we’re already in the home directory (as we landed here after logging in from level 0; if you're not you can use the command `cd ~` to go to the home directory), we can list the files in the directory using:

```bash
ls
```

We should see the `readme` file listed. Now we can use the `cat` command to display its contents:

```bash
cat readme
```

This will output the password for the next level:

```
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

Now we use this password to ssh into the next user, `bandit1`:

```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

When prompted, enter the password:

```
ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

And we’re in! On to the next level.