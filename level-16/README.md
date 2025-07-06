## Bandit Level 16 — Talking Over SSL

### Description

In this level, we’re told that the password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.

### Walkthrough

Let’s start by scanning the open ports on localhost using `nmap`:

```bash
bandit15@bandit:~$ nmap localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-07-06 17:42 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00018s latency).
Not shown: 993 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
1111/tcp  open  lmsocialserver
1840/tcp  open  netopia-vo2
4321/tcp  open  rwhois
8000/tcp  open  http-alt
30000/tcp open  ndmps
50001/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.10 seconds
```
That's weird! because port `30001` isn’t shown, but there’s an open port `50001`. It doesn't hurt us to try to connect to the open port we've found:

```bash
bandit15@bandit:~$ nc localhost 50001
asfaf
Wrong! Please enter the correct current password.
```
Nice, looks like we've got a connection and we can talk to this service; let's try passing the password for the current level:
```bash
bandit15@bandit:~$ nc localhost 50001
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```
That works! But hold up, we were instructed to connect to the port 30001 using SSL/TLS encryption.

SSL/TLS Approach (The Intended Way)
---

Let’s try connecting to `localhost:30001` using `openssl s_client`:

```bash
bandit15@bandit:~$ openssl s_client -connect localhost:30001

CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
---
...
read R BLOCK
```
Hmm, Looks like we’ve made a proper SSL handshake. Let's try some random input:
```bash
sdsf
Wrong! Please enter the correct current password.
closed
```
OK now we're talking (literally)!
One more try with the correct password:
```bash
bandit15@bandit:~$ openssl s_client -connect localhost:30001
CONNECTED(00000003)
Can't use SSL_get_servername
depth=0 CN = SnakeOil
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN = SnakeOil
verify return:1
---
...
read R BLOCK
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
Correct!
kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx
```

### Note
- `openssl` is a robust toolkit for the Transport Layer Security (TLS) and Secure Sockets Layer (SSL) protocols. `s_client` is a subcommand used to act as an SSL/TLS client; It's commonly used for testing and debugging SSL/TLS connections (e.g., HTTPS, SMTPS, LDAPS, etc.).

### What exactly happened

In this level, we were instructed to submit the current password to `localhost` on port `30001` using SSL/TLS encryption. Although a basic nmap scan didn’t reveal this port (likely because the service only responds to proper TLS handshakes or idk some weird reason; you can make sure the port `30001` is open by telling nmap to scan the port specifically: `nmap -p 30001 localhost`), we confirmed it was running by connecting with `openssl s_client`. Sending the password through this encrypted channel returned the next level’s password.\
Although a decoy or fallback service for testing running on port `50001` also accepted the password in plain text (might be left open for convenience; maybe intentionally or not), but that wasn't the intended challenge. But the real challenge here was recognizing the need for TLS and using the correct tool to speak the language.
