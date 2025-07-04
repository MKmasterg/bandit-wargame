
## Bandit Level 8 — Finding a Specific Keyword

### Description

In this level, we’re told that the next password is stored in the file `data.txt`, **next to the word `millionth`**.

### Walkthrough

As usual, we start in the home directory when we log in (if not, use `cd ~` to return to it).  
Reminder: for each level, we log in as a different user:  
`bandit{level_number}@bandit.labs.overthewire.org`.

There’s a file called `data.txt` in the home directory. If we just run `cat data.txt`, we’ll get a massive wall of text — not very helpful.

Instead, we can filter the output using `grep` to search for the word `millionth`:

```bash
bandit7@bandit:~$ cat data.txt | grep "millionth"
millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

And there it is — the password is right next to the word `millionth`.

#### Note

`grep` is a powerful tool used to search for patterns in files. You can even use regular expressions with it! Check the [man page](https://man7.org/linux/man-pages/man1/grep.1.html) to learn more.

And that’s the password for the next level!