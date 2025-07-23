## Bandit Level 28 — Git

### Description

There is a git repository at `ssh://bandit27-git@localhost/home/bandit27-git/repo` via the port `2220`. The password for the user `bandit27-git` is the same as for the user `bandit27`.

Clone the repository and find the password for the next level.

### Walkthrough

First let's create a temp directory
```bash
bandit27@bandit:~$ mktemp -d
/tmp/tmp.48jJwmTDaZ
bandit27@bandit:~$ cd /tmp/tmp.48jJwmTDaZ
```
Now we try to clone the git repository:
```bash
bandit27@bandit:/tmp/tmp.48jJwmTDaZ$ git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
Cloning into 'repo'...
...
bandit27-git@localhost's password:
upsNCc7vzaRDx6oZC6GiR6ERwe1MowGB

remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
```
Check the content of the cloned repository:

```bash
bandit27@bandit:/tmp/tmp.48jJwmTDaZ$ ls
repo
bandit27@bandit:/tmp/tmp.48jJwmTDaZ$ cd repo
bandit27@bandit:/tmp/tmp.48jJwmTDaZ/repo$ ls
README
bandit27@bandit:/tmp/tmp.48jJwmTDaZ/repo$ head README
The password to the next level is: Yz9IpL0sBcCeuG7m9uQFt8ZNpS4HZRcN
```
And we got the password!