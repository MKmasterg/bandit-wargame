## Bandit Level 30 — Git Branches

### Description

There is a git repository at `ssh://bandit29-git@localhost/home/bandit29-git/repo` via the port `2220`. The password for the user `bandit29-git` is the same as for the user `bandit29`.

Clone the repository and find the password for the next level.

### Walkthrough

First, let's create a temp directory:
```bash
bandit29@bandit:~$ mktemp -d
/tmp/tmp.08DZ2GecmQ
bandit29@bandit:~$ cd /tmp/tmp.08DZ2GecmQ
```
Now we clone the git repository:
```bash
bandit29@bandit:/tmp/tmp.08DZ2GecmQ$ git clone ssh://bandit29-git@localhost:2220/home/bandit29-git/repo
Cloning into 'repo'...
...
bandit29-git@localhost's password: <password of bandit29>
```
Move into the repo and inspect the default branch:
```bash
bandit29@bandit:/tmp/tmp.08DZ2GecmQ$ cd repo
bandit29@bandit:/tmp/tmp.08DZ2GecmQ/repo$ ls
README.md
bandit29@bandit:/tmp/tmp.08DZ2GecmQ/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```
No useful password here.

Maybe we could find something in the Git history:

```bash
bandit29@bandit:/tmp/tmp.08DZ2GecmQ/repo$ git log
commit 3b8b91fc3c48f1a19d05670fd45d3e3f2621fcfa (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:21 2025 +0000

    fix username

commit 8d2ffeb5e45f87d0abb028aa796e3ebb63c5579c
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:21 2025 +0000

    initial commit of README.md
```
Let's inspect them:
```bash
bandit29@bandit:/tmp/tmp.08DZ2GecmQ/repo$ git show 8d2ffeb5e45f87d0abb028aa796e3ebb63c5579c
commit 8d2ffeb5e45f87d0abb028aa796e3ebb63c5579c
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:21 2025 +0000

    initial commit of README.md

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..2da2f39
--- /dev/null
+++ b/README.md
@@ -0,0 +1,8 @@
+# Bandit Notes
+Some notes for bandit30 of bandit.
+
+## credentials
+
+- username: bandit29
+- password: <no passwords in production!>
+

bandit29@bandit:/tmp/tmp.08DZ2GecmQ/repo$ git show 3b8b91fc3c48f1a19d05670fd45d3e3f2621fcfa
commit 3b8b91fc3c48f1a19d05670fd45d3e3f2621fcfa (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:21 2025 +0000

    fix username

diff --git a/README.md b/README.md
index 2da2f39..1af21d3 100644
--- a/README.md
+++ b/README.md
@@ -3,6 +3,6 @@ Some notes for bandit30 of bandit.

 ## credentials

-- username: bandit29
+- username: bandit30
 - password: <no passwords in production!>

```
Well I guess nothing could be found here; at least not in this branch. Let's check the others:
```bash
bandit29@bandit:/tmp/tmp.08DZ2GecmQ/repo$ git branch -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```
Switch to the `dev` branch:
```bash
bandit29@bandit:/tmp/tmp.08DZ2GecmQ/repo$ git checkout dev
Switched to a new branch 'dev'
```
Check the contents again:
```bash
bandit29@bandit:/tmp/tmp.08DZ2GecmQ/repo$ ls
README.md  code
bandit29@bandit:/tmp/tmp.08DZ2GecmQ/repo$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: qp30ex3VLz5MDG1n91YowTv4Q8l7CDZL
```
We got the password!