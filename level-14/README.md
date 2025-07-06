## Bandit Level 14 — Logging in with an SSH Private Key

### Description

In this level, we’re told that the password for the next level is stored in `/etc/bandit_pass/bandit14` and can only be read by user **bandit14**. However, we’re not given a password. Instead, we are given a **private SSH key** stored in a file called `~/sshkey.private`.

Our task is to use this key to log in as `bandit14` and try to read the password for the next level.

---

### Walkthrough


Let's try to access the next password directly:

```bash
bandit13@bandit:~$ cd /etc/bandit_pass/
bandit13@bandit:/etc/bandit_pass$ cat bandit14
cat: bandit14: Permission denied
```

No worries. Let's go back to our home directory:

```bash
bandit13@bandit:/etc/bandit_pass$ cd ~
```


Let’s start by listing the contents of the home directory:

```bash
bandit13@bandit:~$ ls
sshkey.private
```

We see the private key file. Let's look inside:

```bash
bandit13@bandit:~$ cat sshkey.private
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxkkOE83W2cOT7IWhFc9aPaaQmQDdgzuXCv+ppZHa++buSkN+
... [omitted for brevity] ...
-----END RSA PRIVATE KEY-----
```

This is a valid RSA private key. We will use it to authenticate as `bandit14`.

Attempting to SSH to bandit14:
```bash
bandit13@bandit:~$ ssh -i ./sshkey.private bandit14@localhost -p 2220
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can't be established.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
... [welcome message] ...
```

Success! We’ve logged in as `bandit14`.

#### Note
For login via ssh private key, use the `-i` option followed by the path to the private key file.

Let’s now try reading the password for the next level:

```bash
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```
We got the password!