---
author: "Justin Perdok"
categories:
  ["ctf", "OverTheWire", "wargames", "Write-Up", "Bandit", "find", "strings"]
date: 2019-09-11T21:16:17Z
description: ""
draft: False
slug: write-up-overthewire-bandit5"
summary: "Lets get cracking with find."
tags:
  ["ctf", "OverTheWire", "wargames", "Write-Up", "Bandit", "find", "strings"]
title: "Write-up: OverTheWire - Bandit Level 5 → Level 7"
---

### Bandit Level 5 → Level 6

**Level Goal:**
*The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:*
- *human-readable*
- *1033 bytes in size*
- *not executable*

Welp in this case I think its time to bust out trusty old `find`. The level goal gives us some clear hints what we can use to filter with `find`.

```
~/wargames/overthewire/bandit$ ssh bandit5@bandit.labs.overthewire.org -p 2220

bandit5@bandit~$ ls
inhere
bandit5@bandit~$ cd inhere/
bandit5@bandit~/inhere$ ls -la
total 88
drwxr-x--- 22 root bandit5 4096 Oct 16  2018 .
drwxr-xr-x  3 root root    4096 Oct 16  2018 ..
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere00
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere01
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere02
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere03
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere04
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere05
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere06
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere07
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere08
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere09
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere10
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere11
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere12
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere13
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere14
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere15
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere16
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere17
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere18
drwxr-x---  2 root bandit5 4096 Oct 16  2018 maybehere19

```

So `find` is pretty handy tool. `find` searches one or more directory trees of a file system, it then locates files based on user-specified filters and can even apply a user-specified action on each matched file!

So we know that the file is somewhere in the folder `inhere`. Since we already `cd` into this folder we can simply supply a dot `.` to `find`. A dot `.` simply represents the current directory.

We also know from the level goal that this is a file, so we use the `-type` flag. By specifying `f` on the `-type` flag where are telling find to search for files.

Next thing on the list is `human-readable` text. `find` has a `-readable` flag you can supply, this however does not filter for human-readable. It simply matches files which are readable by the current user. We can however use `strings` later to filter for `human-readable` text.

Another thing we know is that password file is not executable. `find` has a flag for that, `-executable`. If we supply it like that it will filter for files that are executable. We don't want that, we can fix this by using the `!` operator.

Last thing is file size, the level goal tells us the password file is 1033 bytes. Again `find` has a flag for that, `-size`. The `-size` flag has multiple suffixes, we can use `c` to specify that our number is in bytes.

That should cover our filter. Now for our action. We can use the `-exec` flag for this. You should probably read the `find` man page to read up on `-exec`. In our case we want to `cat` each file that find finds.

So after all that we have the following command:

```
find . -type f -readable ! -executable -size 1033c -exec cat {} \;

```

To ensure the output only contains `human-readable` text we can simply pipe `|` this to `strings`.

```
bandit5@bandit~/inhere$ find . -type f -readable ! -executable -size 1033c -exec cat {} \; | strings
DXjZPULLxYr17uwoI01bNLQbtFemEgo7

bandit5@bandit~/inhere$ logout
Connection to bandit.labs.overthewire.org closed.

```

### Bandit Level 6 → Level 7

**Level Goal:**
*The password for the next level is stored somewhere on the server and has all of the following properties:

    owned by user bandit7
    owned by group bandit6
    33 bytes in size

*

So lets check out home folder... ow wait... no file. In that case, lets just use `find` on the root of the file structure!

```
~/wargames/overthewire/bandit$ ssh bandit6@bandit.labs.overthewire.org -p 2220


bandit6@bandit~$ ls -lA
total 20
-rw-r--r--  1 root root  220 May 15  2017 .bash_logout
-rw-r--r--  1 root root 3526 May 15  2017 .bashrc
-rw-r--r--  1 root root  675 May 15  2017 .profile
bandit6@bandit~$ cd /
```

We already know how to specify the file size and location. To specify a user or group we can use the `-group` and `-user` flags.

```
bandit6@bandit:/$ find . -size 33c -group bandit6 -user bandit7 -readable -exec cat {} \; | strings
find: ‘./run/lvm’: Permission denied
find: ‘./run/screen/S-bandit16’: Permission denied
find: ‘./run/screen/S-bandit26’: Permission denied
```

To avoid all these `Permission denied` being returned to our shell we can redirect a thing called `stderr` to a place called `/dev/null`.

`stderr` , also known as standard error, is the default file descriptor where a process can write error messages.

`/dev/null` , also known as the null device, is a device file that discards all data written to it but reports that the write operation succeeded.

We can redirect `stderr` to `/dev/null` with `2>/dev/null`. When we run it with this redirection we simply get the password.

```
bandit6@bandit/$ find . -size 33c -group bandit6 -user bandit7 2>/dev/null -exec cat {} \; | strings
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

bandit6@bandit/$ logout
Connection to bandit.labs.overthewire.org closed.
```
