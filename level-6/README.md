## Bandit Level 6 — Finding Files by Properties

### Description

In this level, we’re told that the next password is stored in a specific file inside the `inhere` directory. The file has the following properties:

-   It’s human-readable
    
-   It’s **1033 bytes** in size
    
-   It’s **not executable**
    

### Walkthrough

As usual, we start in the home directory when we log in (if not, use `cd ~` to return to it).  
Reminder: for each level, we log in as a different user:  
`bandit{level_number}@bandit.labs.overthewire.org`.

Let’s check what’s inside the home directory:

```bash
bandit5@bandit:~$ ls
inhere
```

We see the `inhere` directory, so let’s go into it:
```bash
bandit5@bandit:~$ cd inhere/
```

List its contents:

```bash
bandit5@bandit:~/inhere$ ls
maybehere00/  maybehere01/  maybehere02/  maybehere03/  maybehere04/  maybehere05/  maybehere06/  
maybehere07/  maybehere08/  maybehere09/  maybehere10/  maybehere11/  maybehere12/  maybehere13/  
maybehere14/  maybehere15/  maybehere16/  maybehere17/  maybehere18/  maybehere19/
```

Wow — lots of directories! Checking every file manually would be exhausting, so we’ll use the `find` command to help us locate the file that matches the given criteria.

```bash
bandit5@bandit:~/inhere$ find . -size 1033c ! -executable -readable
./maybehere07/.file2
```

Here’s what the flags mean:

-   `-size 1033c`: Find files with a size of exactly 1033 **bytes** (`c` stands for bytes).
    
-   `! -executable`: Excludes executable files.
    
-   `-readable`: Only include files that we can read.
    

#### Note

`find` is a powerful command for locating files in a directory hierarchy. You can learn more by reading its [man page](https://man7.org/linux/man-pages/man1/find.1.html).

From the output, we see that `.file2` inside `maybehere07` matches the criteria.

Let’s read the file:

```bash
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

And that’s the password for the next level!