## Bandit Level 27 — Bandit27-do

### Description

Good job getting a shell! Now hurry and grab the password for bandit27!

### Walkthrough

Hope you didn't lose the shell you got from the previous level! If so follow the walkthrough from the previous level.

When you got the shell, you should see something like this:
```bash
bandit26@bandit:~$ ls
bandit27-do  text.txt
```
Let's run `bandit27-do`:
```bash
bandit26@bandit:~$ ./bandit27-do
Run a command as another user.
  Example: ./bandit27-do id
```

Great let's get the password then:

```bash
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB
```