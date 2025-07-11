## Bandit Level 20 — Borrowed Identity

### Description

To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (`/etc/bandit_pass`), after you have used the setuid binary.


### Walkthrough

#### What is a setuid binary?
A setuid binary is a file that, when executed, runs with the permissions of its **owner**, not the person executing it. In our case the owner of the `bandit20-do` is owned by `bandit20`:
```bash
bandit19@bandit:~$ ls -l
-rwsr-x--- 1 bandit20 bandit19 15424 Apr 10 14:23 bandit20-do
```
- `rws` — the owner (`bandit20`) has read, write, and setuid-execute (`s`) permissions.

- The binary belongs to `bandit20` but is executable by `bandit19` — us.

- That means if we run it, it behaves as if it’s `bandit20` running the command.

Let's run `bandit20-do`:
```bash
bandit19@bandit:~$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id
```
We can test it:

```bash
bandit19@bandit:~$ ./bandit20-do whoami
bandit20
```
Perfect! We’re now executing commands as `bandit20`.

So to read the password:

```bash
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```
Excellent and we're done!