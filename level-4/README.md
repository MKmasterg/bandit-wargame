## Bandit Level 4 — Listing hidden files

### Description

In this level, we’re told the next password is in a hidden file located inside the `inhere` directory in the home directory.

### Walkthrough

Since we’re already in the home directory (as we landed here after logging in from the previous level; if you're not you can use the command `cd ~` to go to the home directory. please note that for each level we should log into another user of the host domain: bandit{level_number}@bandit.labs.overthewire.org), we can list the files and directories in the current directory using:

```bash
ls
```

We should see the `inhere` directory listed; we navigate into it using:

```bash
cd inhere
```

Now if we run a regular `ls`:

```bash
ls
```
...nothing shows up! Suspicious. That’s because the file is hidden.

To list hidden files, we need to use the `-a` flag:
```bash
ls -a
```
#### Note
In Unix-like systems, files starting with a `.` are considered hidden. If you check the [man page of ls](https://linuxcommand.org/lc3_man_pages/ls1.html) the `-a` flag tells `ls` to show all files, including those starting with a dot.

Now we see a file named `...Hiding-From-You`!

Let’s read it using `cat`:
```bash
cat ...Hiding-From-You
```
The output is:
```
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

And there’s the password for the next level!

