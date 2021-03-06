---
author: "Justin Perdok"
categories:
  [
    "Bandit",
    "OverTheWire",
    "wargames",
    "Write-Up",
    "ctf",
    "nmap",
    "diff",
    "ssh",
    "sftp",
    "scp",
  ]
date: 2019-09-12T11:51:54Z
description: ""
draft: false
slug: "write-up-overthewire-bandit-level-16-level-level-20"
summary: "nmap all the things."
tags:
  [
    "Bandit",
    "OverTheWire",
    "wargames",
    "Write-Up",
    "ctf",
    "nmap",
    "diff",
    "ssh",
    "sftp",
    "scp",
  ]
title: "Write-up: OverTheWire - Bandit Level 16 → Level 20"
---

### Bandit Level 16 → Level 17

**Level Goal:**
*The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.*

Level 16 is similar to 14 and 15, only this time we are given a range of 1000 ports. We could write a bash script that loops over each possible port, but why bother if we can use a tool like `nmap` to scan for open ports.

We tell `nmap` to target host `127.0.0.1` and scan all the ports in the range of `31000-32000` by using the `-p` flag. This results in 2 open ports.

```
~/wargames/overthewire/bandit$ ssh bandit16@bandit.labs.overthewire.org -p 2220

bandit16@bandit~$ nmap 127.0.0.1 -p 31000-32000

Starting Nmap 7.40 ( https://nmap.org ) at 2019-09-11 13:57 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00024s latency).
Not shown: 999 closed ports
PORT      STATE SERVICE
31518/tcp open  unknown
31790/tcp open  unknown

Nmap done: 1 IP address (1 host up) scanned in 0.08 seconds
```

Lets connect to the first port using `s_client` . If we submit something it returns the same string. So lets cancel out of this connection and try the next port.

```
bandit16@bandit~$ openssl s_client --connect 127.0.0.1:31518 -quiet
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
cluFn7wTiGryunymYOu4RcffSxQluehd
cluFn7wTiGryunymYOu4RcffSxQluehd
^C
```

There we go.

```
bandit16@bandit~$ openssl s_client --connect 127.0.0.1:31790 -quiet
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
cluFn7wTiGryunymYOu4RcffSxQluehd
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

bandit16@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 17 → Level 18

**Level Goal:**
*There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19
*

First things first, lets setup the private key from Level 16 and connect to the server.

```
~/wargames/overthewire/bandit$ mkdir bandit17
~/wargames/overthewire/bandit$ vim bandit17/id_rsa
~/wargames/overthewire/bandit$ chmod 0400  bandit17/id_rsa
~/wargames/overthewire/bandit$ ssh bandit17@bandit.labs.overthewire.org -p 2220 -i bandit17/id_rsa
```

Here we find the 2 passwords files. Since only 1 line has been changed lets use `diff` to search for difrences between these files.

```
bandit17@bandit~$ ls
passwords.new  passwords.old
bandit17@bandit~$ diff passwords.new passwords.old
42c42
< kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
---
> hlbSBPAWJmL6WFDb06gpTx1pPButblOA
bandit17@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 18 → Level 19

**Level Goal:**
*The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.
*

So whenever we login with SSH the session is logged of immediately.

```
~/wargames/overthewire/bandit$ ssh bandit18@bandit.labs.overthewire.org -p 2220
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:
Byebye !
Connection to bandit.labs.overthewire.org closed.
```

What we can do to bypass this is to pass use the `-t` flag.

> -t Force pseudo-terminal allocation. This can be used to execute arbitrary screen-based programs on a remote machine, which can be very useful, e.g. when implementing menu services. Multiple -t options force tty allocation, even if ssh has no local tty.

```
~/wargames/overthewire/bandit$ ssh bandit18@bandit.labs.overthewire.org -p 2220 -t "ls"
bandit18@bandit.labs.overthewire.org's password:
readme
~/wargames/overthewire/bandit$ ssh bandit18@bandit.labs.overthewire.org -p 2220  -t "cat readme"
bandit18@bandit.labs.overthewire.org's password:
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
```

Instead of running `ls` and `cat` we also could have started `bash` with the `--norc` flag.

> When an interactive shell that is not a login shell is started, bash reads and executes commands from ~/.bashrc, if that file exists. This may be inhibited by using the --norc option.

```
~/wargames/overthewire/bandit$ ssh bandit18@bandit.labs.overthewire.org -p 2220 -t "bash --norc"
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:
bash-4.4$ ls
readme
bash-4.4$ cat readme
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
```

We also could have used `sftp` or `scp` to copy the file to our local storage and `cat` it there.

```
~/wargames/overthewire/bandit$ sftp -P 2220 bandit18@bandit.labs.overthewire.org:readme a
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password:
Connected to bandit.labs.overthewire.org.
Fetching /home/bandit18/readme to a

~/wargames/overthewire/bandit$ cat a
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
```

### Bandit Level 19 → Level 20

**Level Goal:**
*To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.
*

The level goal explains it all. This binary runs the command you specify as the user bandit20.

```
~/wargames/overthewire/bandit$ ssh bandit19@bandit.labs.overthewire.org -p 2220
bandit19@bandit~$ ls
bandit20-do
bandit19@bandit~$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit~$ ./bandit20-do cat /etc/bandit_pass/bandit20
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```
