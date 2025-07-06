## Bandit Level 15 — Talking to a Port

### Description

In this level, we’re told that the password for the next level can be retrieved by submitting the password of **the current level** to port `30000` on `localhost`.

### Walkthrough
First we need to now what service is running on port `30000` and then we can find a way to communicate with it. In order to do so, we run `nmap` on `localhost`:

```bash
bandit14@bandit:~$ nmap localhost
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-07-06 17:26 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00012s latency).
Not shown: 993 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
1111/tcp  open  lmsocialserver
1840/tcp  open  netopia-vo2
4321/tcp  open  rwhois
8000/tcp  open  http-alt
30000/tcp open  ndmps
50001/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.05 seconds
```
Beautiful! We can see the `ndmps` service is running on port `30000`.

Now we need to find a way to communicate with this service. We just need to send the password for the current level to this port. The simplest way to do this is with `netcat` (or `nc`), a tool for making raw TCP/UDP connections:

```bash
bandit14@bandit:~$ nc localhost 30000

```
When connected we see no messages and our cursor is waiting for input; we simply paste the password for the current level:
```bash
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
Correct!
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```
And that's our password!

### Note
- `nmap` is a powerful tool for network exploration and security auditing. It can be used to discover hosts and services on a computer network, thus helping us identify potential points of interaction.
- `netcat` (or `nc`) is a versatile networking tool that can be used for reading from and writing to network connections using TCP or UDP. It is often used for debugging and investigating the network.