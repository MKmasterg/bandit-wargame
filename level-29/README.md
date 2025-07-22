## Bandit Level 29 — git log

### Description

There is a git repository at `ssh://bandit28-git@localhost/home/bandit28-git/repo` via the port `2220`. The password for the user `bandit28-git` is the same as for the user `bandit28`.

Clone the repository and find the password for the next level.

### Walkthrough

First, let's create a temp directory:
```bash
bandit28@bandit:~$ mktemp -d
/tmp/tmp.NMFW1ssNBk
bandit28@bandit:~$ cd /tmp/tmp.NMFW1ssNBk
```
Now we clone the git repository:
```bash
bandit28@bandit:/tmp/tmp.NMFW1ssNBk$ git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
Cloning into 'repo'...
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
...
bandit28-git@localhost's password: (Enter the password for bandit28)
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 9 (delta 2), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (2/2), done.
```

List the contents:
```bash
bandit28@bandit:/tmp/tmp.NMFW1ssNBk$ ls
repo
bandit28@bandit:/tmp/tmp.NMFW1ssNBk$ cd repo/
bandit28@bandit:/tmp/tmp.NMFW1ssNBk/repo$ ls
README.md
```

Check the `README`:
```bash
bandit28@bandit:/tmp/tmp.NMFW1ssNBk/repo$ head README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```
Looks like the password is hidden. Let's check the Git history:
```bash
bandit28@bandit:/tmp/tmp.NMFW1ssNBk/repo$ git log
commit 674690a00a0056ab96048f7317b9ec20c057c06b (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    fix info leak

commit fb0df1358b1ff146f581651a84bae622353a71c0
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    add missing data

commit a5fdc97aae2c6f0e6c1e722877a100f24bcaaa46
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    initial commit of README.md

```
Interesting, maybe if we could check the contents of the previous commits we could find the password.
So now we inspect the previous commit:
```bash
bandit28@bandit:/tmp/tmp.NMFW1ssNBk/repo$ git show fb0df1358b1ff146f581651a84bae622353a71c0
commit fb0df1358b1ff146f581651a84bae622353a71c0
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Apr 10 14:23:19 2025 +0000

    add missing data

diff --git a/README.md b/README.md
index 7ba2d2f..d4e3b74 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials

 - username: bandit29
-- password: <TBD>
+- password: 4pT1t5DENaYuqnqvadYs1oE4QLCdjmJ7
```
And there's the password!