## Bandit Level 32 — git push

### Description

There is a git repository at `ssh://bandit31-git@localhost/home/bandit31-git/repo` via the port `2220`. The password for the user `bandit31-git` is the same as for the user `bandit31`.

Clone the repository and find the password for the next level.

### Walkthrough

First, let's create a temp directory:
```bash
bandit31@bandit:~$ mktemp -d
/tmp/tmp.4S39o36RDk
bandit31@bandit:~$ cd /tmp/tmp.4S39o36RDk
```
Now we clone the git repository:
```bash
bandit31@bandit:/tmp/tmp.4S39o36RDk$ git clone ssh://bandit31-git@localhost:2220/home/bandit31-git/repo
Cloning into 'repo'...
...
bandit31-git@localhost's password: <password for bandit31>
```
Move into the repo and inspect the default branch:
```bash
bandit31@bandit:/tmp/tmp.4S39o36RDk$ cd repo
bandit31@bandit:/tmp/tmp.4S39o36RDk/repo$ git branch
* master
bandit31@bandit:/tmp/tmp.4S39o36RDk/repo$ ls
README.md
bandit31@bandit:/tmp/tmp.4S39o36RDk/repo$ cat README.md
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master

```
OK let's do that:

```bash
bandit31@bandit:/tmp/tmp.4S39o36RDk/repo$ echo "May I come in?" > key.txt
bandit31@bandit:/tmp/tmp.4S39o36RDk/repo$ git add key.txt
The following paths are ignored by one of your .gitignore files:
key.txt
hint: Use -f if you really want to add them.
hint: Turn this message off by running
hint: "git config advice.addIgnoredFile false"
```
Looks like the `.txt` files are being ignored because of the content of `.gitignore`.
We can force add it by passing `-f` flag:
```bash
bandit31@bandit:/tmp/tmp.4S39o36RDk/repo$ git add -f key.txt
```
Nice and all we have to do is commit and push:
```bash
bandit31@bandit:/tmp/tmp.4S39o36RDk/repo$ git commit -m "Add key.txt"
[master fd190ab] Add key.txt
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
```
And push:
```bash
bandit31@bandit:/tmp/tmp.4S39o36RDk/repo$ git push
...
bandit31-git@localhost's password: <password for bandit31>
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 2 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 325 bytes | 325.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote: ### Attempting to validate files... ####
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
remote: Well done! Here is the password for the next level:
remote: 3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
To ssh://localhost:2220/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://localhost:2220/home/bandit31-git/repo'
```
Success! This is the password:
```bash
3O9RfhqyAlVBEZpVb6LYStshZoqoSx5K
```