## Bandit Level 19 — Bypassing the Trap

### Description

The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified `.bashrc` to log you out when you log in with SSH.

### Walkthrough

By the description we can understand that we should have truoble when loging in:

```bash
# ssh bandit18@bandit.labs.overthewire.org -p 2220
                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:
```
And when the password entered:

```bash
...
--[ More information ]--

  For more information regarding individual wargames, visit
  http://www.overthewire.org/wargames/

  For support, questions or comments, contact us on discord or IRC.

  Enjoy your stay!

Byebye !
Connection to bandit.labs.overthewire.org closed.
```
We’re immediately kicked out. Looks like `.bashrc` has been set up to terminate the session as soon as we connect.
#### What is `.bashrc`
`.bashrc` is a shell script that Bash runs whenever you start a new interactive shell session (like openning a terminal). It is used to configure the shell environment, set up aliases, and run startup programs. It's usually in the home directory.\
**Note:** For login shells (like when you SSH into a server), `.bash_profile`, `.bash_login`, or `.profile` are run instead, but they often call `.bashrc` inside them to keep things consistent.

#### What can we do?
We have two ways to obtain our password:

1. **Use SSH to run a single command:** When you run a single command over SSH, `.bashrc` is usually not sourced, because it's not an interactive shell. It’s also typically not a login shell either. So your aliases, functions, or environment variables in `.bashrc` won’t apply unless you explicitly source it.

2. **Use a different shell:** Instead of using the default shell, we can try using a different shell (like `sh` or `dash`) that doesn't have the same logout behavior. This might allow us to access the server without being logged out.

---
#### Approach 1
We can simply try to read the file using a command like this:
```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 "/bin/cat readme"

                         _                     _ _ _
                        | |__   __ _ _ __   __| (_) |_
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_
                        |_.__/ \__,_|_| |_|\__,_|_|\__|


                      This is an OverTheWire game server.
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```
Nice we got the password!

#### Approach 2
I like this approach because it allows us to bypass the logout behavior entirely. By using a different shell, we can avoid the issues caused by the `.bashrc` configuration.

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 -t "/bin/sh""
```
- `-t` is for allocating a pseudo-terminal, which can help in situations where the shell expects a terminal environment.

And voila! We can now access the server without being logged out:

```bash
$ ls
readme
$ cat readme
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8
```
