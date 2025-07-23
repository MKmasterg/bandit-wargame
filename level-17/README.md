## Bandit Level 17 — Port Hunt with TLS

### Description

The credentials for the next level can be retrieved by submitting the password of the **current level** to a port on `localhost` in the range `31000` to `32000`. First find out which of these ports have a server listening on them. Then find out which of those speak **SSL/TLS** and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

### Walkthrough

Let’s start by scanning all ports in the specified range using `nmap` :

```bash
bandit16@bandit:~$ nmap -p 31000-32000 localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-07-08 17:14 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00010s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.05 seconds
```
We’ve found five open ports. Now let’s figure out which of these support SSL/TLS:

```bash
bandit16@bandit:~$ nmap --script ssl-enum-ciphers -p 31046,31518,31691,31790,31960 localhost
```
- `--script ssl-enum-ciphers` is an Nmap script that attempts to determine the SSL/TLS ciphers supported by a server.

If we look closely we can see only ports `31518` and `31790` respond with valid SSL cipher information — which means they speak SSL/TLS. Now we need to test both of them:
```bash
bandit16@bandit:~$ echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | openssl s_client -connect localhost:31518 -quiet
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```
No luck here — it just echoed back what we sent.
Let’s try the next one:
```bash
bandit16@bandit:~$ echo "kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx" | openssl s_client -connect localhost:31790 -quiet
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
-----[..................]-----
-----END RSA PRIVATE KEY-----
```
Perfect! We got a private key for the next user: `bandit17`.

Let’s save it and use it to log in:
```bash
bandit16@bandit:~$ mktemp -d
/tmp/tmp.UYLCsJwaHg
bandit16@bandit:~$ cd /tmp/tmp.UYLCsJwaHg
bandit16@bandit:/tmp/tmp.UYLCsJwaHg$ vim secretKey
```
#### Note
You can use any text editor you want; I used vim, in order to paste a text first you need to go in `insert` mode by pressing `i`. Then just `ctrl + v` or `ctrl + shift + v` depending on the terminal you're using. After that, simply press `esc` and type `:wq` and hit enter.

Now we `ssh`:
```bash
bandit16@bandit:/tmp/tmp.UYLCsJwaHg$ ssh -i secretKey bandit17@bandit.labs.overthewire.org -p 2220
```
Interestingly we got a warning:
```bash
...
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0664 for 'secretKey' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```
This forces us to change the key’s permissions; to do so, just type this command:
```bash
bandit16@bandit:/tmp/tmp.UYLCsJwaHg$ chmod 400 secretKey
```
- `chmod 400` ensures that only you can read the private key.

And try again:
```bash
bandit16@bandit:/tmp/tmp.UYLCsJwaHg$ ssh -i secretKey bandit17@bandit.labs.overthewire.org -p 2220
```
Nice we got in:
```bash
bandit17@bandit:~$
```

OK now, for reading the password for the next level we should just `cat` the `bandit17` at `/etc/bandit_pass` dierctory:
```bash
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
EReVavePLFHtFlFsjn3hyzMlvSuSAcRD
```
And we're done!