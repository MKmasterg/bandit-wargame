## Bandit Level 5 — Identifying Human-Readable Files

### Description

In this level, we’re told that the next password is stored in a _human-readable_ file inside the `inhere` directory, located in the home directory.

### Walkthrough

As usual, we land in the home directory when we log in (if not, use `cd ~` to get there).  
Reminder: for each level, we log in as a different user: `bandit{level_number}@bandit.labs.overthewire.org`.

Let’s list the contents of the home directory:

```bash
bandit4@bandit:~$ ls
inhere
```

We see the `inhere` directory — let’s navigate into it:

```bash
bandit4@bandit:~$ cd inhere/
```

Now list the files:

```bash
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
```

We’re faced with a bunch of files!  
The naive approach would be to `cat` each file and look for a human-readable one... but that’s time-consuming.

Luckily, we have a tool called `file` that helps us identify file types:

```bash
bandit4@bandit:~/inhere$ file ./*
./-file00: PGP Secret Sub-key -
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data

```

The `*` wildcard means “all files in the current directory.”

#### Note

`file` is a powerful command used to determine file types. According to the [man page](https://www.man7.org/linux/man-pages/man1/file.1.html), it runs a series of tests to identify file formats — super useful when you don’t know what you’re dealing with.

From the output, we see that `-file07` is an ASCII text file — that’s likely the one we want.

Let’s read it:

```bash
bandit4@bandit:~/inhere$ cat ./-file07
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

And there’s the password for the next level!