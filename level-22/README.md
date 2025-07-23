## Bandit Level 22 — Cronjob Discovery

### Description

A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in `/etc/cron.d/` for the configuration and see what command is being executed.


### Walkthrough

Let’s head into the cron configuration directory:
```bash
bandit21@bandit:~$ cd /etc/cron.d
bandit21@bandit:/etc/cron.d$ ls -l
total 28
-rw-r--r-- 1 root root 123 Apr 10 14:16 clean_tmp
-rw-r--r-- 1 root root 120 Apr 10 14:23 cronjob_bandit22
-rw-r--r-- 1 root root 122 Apr 10 14:23 cronjob_bandit23
-rw-r--r-- 1 root root 120 Apr 10 14:23 cronjob_bandit24
-rw-r--r-- 1 root root 201 Apr  8  2024 e2scrub_all
-rwx------ 1 root root  52 Apr 10 14:24 otw-tmp-dir
-rw-r--r-- 1 root root 396 Jan  9  2024 sysstat
```
We see a file named `cronjob_bandit22`, let's inspect it:
```bash
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```
Let's break this down:
- The script `/usr/bin/cronjob_bandit22.sh` is run every minute (`* * * * *`) and also on every reboot (`@reboot`).
- It runs as user `bandit22`.
- All output is discarded (`&> /dev/null`).

Let’s read the actual script being executed:
```bash
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```
It **copies the password for bandit22 into a temporary file**. The file has the following permissions:
| Who        | Permissions          |
| ---------- | -------------------- |
| **Owner**  | read + write (`rw-`) |
| **Group**  | read only (`r--`)    |
| **Others** | read only (`r--`)    |

That means we can easily read the file:
```bash
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```
And we got the password!
