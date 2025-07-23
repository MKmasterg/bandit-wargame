## Bandit Level 31 — Git Tags

### Description

There is a git repository at `ssh://bandit30-git@localhost/home/bandit30-git/repo` via the port `2220`. The password for the user `bandit30-git` is the same as for the user `bandit30`.

Clone the repository and find the password for the next level.

### Walkthrough

First, let's create a temp directory:
```bash
bandit30@bandit:~$ mktemp -d
/tmp/tmp.XlyWGJycAg
bandit30@bandit:~$ cd /tmp/tmp.XlyWGJycAg
```
Now we clone the git repository:
```bash
bandit30@bandit:/tmp/tmp.XlyWGJycAg$ git clone ssh://bandit30-git@localhost:2220/home/bandit30-git/repo
Cloning into 'repo'...
...
bandit30-git@localhost's password: <password for bandit30>
```
Move into the repo and inspect the default branch:
```bash
bandit30@bandit:/tmp/tmp.XlyWGJycAg$ cd repo
bandit30@bandit:/tmp/tmp.XlyWGJycAg/repo$ ls
README.md
bandit30@bandit:/tmp/tmp.XlyWGJycAg/repo$ cat README.md
just an epmty file... muahaha
```
Looks like a decoy. Let’s inspect the Git history:

```bash
bandit30@bandit:/tmp/tmp.XlyWGJycAg/repo$ git log
commit fb05775f973256dc6d8d5bb6a8e6b96b0d8795c8 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:24 2025 +0000

    initial commit of README.md
```
Nothing useful in the commit message or diff:
```bash
bandit30@bandit:/tmp/tmp.XlyWGJycAg/repo$ git show fb05775f973256dc6d8d5bb6a8e6b96b0d8795c8
...
+just an epmty file... muahaha
```
Let’s check for any tags, which can sometimes be used to hide information:
```bash
bandit30@bandit:/tmp/tmp.XlyWGJycAg/repo$ git tag
secret
```
Found a tag! View its contents:
```bash
bandit30@bandit:/tmp/tmp.XlyWGJycAg/repo$ git show secret
fb5S2xb7bRyFmAvQYQGEqsbhVyJqhnDy
```
Success! This is the password.

#### Note
- **Git tags** are references to specific points in a repository’s history — typically used to mark releases or important commits, like "v1.0", "v2.0", etc. They're kind of like bookmarks, but permanent and intended to be meaningful — especially in projects where you release versions of software.