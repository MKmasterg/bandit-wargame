## Bandit Level 12 — Decoding ROT13

### Description

In this level, we’re told that the next password is stored in the file `data.txt`, where all lowercase (`a–z`) and uppercase (`A–Z`) letters have been rotated by **13 positions**.

### Walkthrough

As usual, we start in the home directory when we log in (if not, use `cd ~` to return to it).
Reminder: for each level, we log in as a different user:
`bandit{level_number}@bandit.labs.overthewire.org`.

There’s a file called `data.txt`. If we read it with `cat`, we see a line of gibberish — but it looks like a ROT13-encoded string:

```bash
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
```

To decode it, we can use the `tr` command to apply a ROT13 transformation:

```bash
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

#### Note

`tr` is a command-line utility for translating or deleting characters. Here, we're using it to apply the **ROT13 cipher**, which shifts each letter by 13 positions in the alphabet.

* `'A-Za-z'` matches all uppercase and lowercase letters.
* `'N-ZA-Mn-za-m'` tells `tr` how to map each character: A → N, B → O, ..., N → A, etc.

This is a very simple substitution cipher — applying it twice gets you back to the original message.

And that’s the password for the next level!