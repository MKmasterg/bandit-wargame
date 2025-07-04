## Bandit Level 9 — Finding a Unique Line

### Description

In this level, we’re told that the next password is stored in `data.txt` and is the **only line of text that occurs exactly once**.

### Walkthrough

As usual, we start in the home directory when we log in (if not, use `cd ~` to return to it).  
Reminder: for each level, we log in as a different user:  
`bandit{level_number}@bandit.labs.overthewire.org`.

We see a file called `data.txt`. If we just run `cat data.txt`, we’ll get a huge wall of text — it’s hard to tell which line is unique by just looking.

Instead, we can use the combination of `sort` and `uniq` to filter out lines that appear only once:

```bash
bandit8@bandit:~$ sort data.txt | uniq -u
4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

There it is — the only line that occurs once. That’s our password!

#### Note

-   `sort` arranges the lines in order so that duplicates are adjacent.
    
-   `uniq -u` filters out only the **unique** lines (i.e., lines that occur once).
    

Together, they form a powerful combo for working with repeated or unique data. You can learn more by checking out their man pages (`man sort`, `man uniq`) or reading online.

And that’s the password for the next level!