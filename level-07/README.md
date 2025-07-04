## Bandit Level 7 — Searching by Ownership and Size

### Description

In this level, we’re told that the next password is stored in a specific file _somewhere_ on the server. The file has the following properties:
    
-   It’s **33 bytes** in size
    
-   It’s **owned by user `bandit7`**
    
-   It’s **owned by group `bandit6`**
    

### Walkthrough

As usual, we start in the home directory when we log in (if not, use `cd ~` to return to it).  
Reminder: for each level, we log in as a different user:  
`bandit{level_number}@bandit.labs.overthewire.org`.

From what we learned in the last level, we can jump straight into using the `find` command. Since the file could be _anywhere_ on the system, we’ll start from the root `/`:

```bash
bandit6@bandit:~$ find / -size 33c -user bandit7 -group bandit6
```

We’ll likely get a lot of permission errors like:

```
find: ‘/root’: Permission denied
find: ‘/proc/tty/driver’: Permission denied
...

```

That’s fine — we can filter out those noisy error messages. In Linux, `2` refers to the **stderr stream** (standard error, [read more](https://en.wikipedia.org/wiki/Standard_streams#Standard_error_%28stderr%29)), and `/dev/null` is a special device that discards everything sent to it.

So we’ll redirect errors to nowhere:

```bash
bandit6@bandit:~$ find / -size 33c -user bandit7 -group bandit6 2>/dev/null
/var/lib/dpkg/info/bandit7.password
```

#### Explanation of flags:

-   `/`: Search from the root directory
    
-   `-size 33c`: Find files that are exactly 33 bytes (`c` = bytes)
    
-   `-user bandit7`: Owned by user `bandit7`
    
-   `-group bandit6`: Owned by group `bandit6`
    
-   `2>/dev/null`: Hide all error messages
    

From the output, we see that the file `/var/lib/dpkg/info/bandit7.password` matches all our criteria.

Let’s read it:

```bash
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

And that’s the password for the next level!