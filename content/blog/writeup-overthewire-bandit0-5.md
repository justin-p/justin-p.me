---
author: "Justin Perdok"
categories:
  ["OverTheWire", "Write-Up", "Bandit", "wargames", "ctf", "ls", "cat"]
date: 2019-09-11T21:04:47Z
description: ""
draft: false
slug: "writeup-overthewire-bandit0-5"
summary: "This blog is still pretty empty, so lets do something about that. Let's create a write-up of the Bandit Wargame."
tags: ["OverTheWire", "Write-Up", "Bandit", "wargames", "ctf", "ls", "cat"]
title: "Write-up: OverTheWire - Bandit level 0 → Bandit level 5"
---

### Bandit Level 0

**Level Goal:**
*The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.*

As straight forward as it comes. Lets connect to that box.

```
~/wargames/overthewire/bandit$ ssh bandit0@bandit.labs.overthewire.org -p 2220
bandit0@bandit~$
```

### Bandit Level 0 → Level 1

**Level Goal:**
*The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.*

Still no surprises here. We start with a simple `ls` .

```
bandit0@bandit~$ ls
readme
```

Well looky here. A file. Let's look whats inside with `cat`.

```
bandit0@bandit~$ cat readme
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

Since we got what we came for lets logoff.

```
bandit0@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 1 → Level 2

**Level Goal:**
*The password for the next level is stored in a file called - located in the home directory*

Lets logon and start looking around.

```
~/wargames/overthewire/bandit$ ssh bandit1@bandit.labs.overthewire.org -p 2220
bandit1@bandit~$ ls
-
```

To `cat` this file we need to escape the `-` character. If you are like my you where probably already mashing tab for the automagic tab autocomplete. This does not work, though we can simply use `./` to escape `-`.

```
bandit1@bandit~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
bandit1@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 2 → Level 3

**Level Goal:**
*The password for the next level is stored in a file called spaces in this filename located in the home directory*

```
~/wargames/overthewire/bandit$ ssh bandit2@bandit.labs.overthewire.org -p 2220

bandit2@bandit~$ ls
spaces in this filename
```

In order to `cat` this file we need to escape the `spaces` somehow.

```
bandit2@bandit~$ cat spaces in this filename
cat: spaces: No such file or directory
cat: in: No such file or directory
cat: this: No such file or directory
cat: filename: No such file or directory
```

We can use quoting for this. Quoting can used to remove the special meaning of certain characters or words to the shell. If you lookup the [man page](https://linux.die.net/man/1/bash) of bash (or type `man bash`) you learn that there are three quoting mechanisms: the escape character, single quotes, and double quotes. Below is an example using double qoutes.

```
bandit2@bandit:~$ cat "spaces in this filename"
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

Another way is using a `non-qouted backslash`. If you are like my you probably already used this solution by using tab autocomplete.

```
bandit2@bandit~$ cat spaces\ in\ this\ filename
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
bandit2@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 3 → Level 4

**Level Goal:**
*The password for the next level is stored in a hidden file in the inhere directory.*

Here we need to `cd` into a sub folder called `inhere` as explained in the level goal. Our goal is to open the a hidden file. By default `ls` does not show hidden files. To show hidden files we can use the `-a` flag (or `-A` if you don't like seeing the implied . and ..).

I personally also use the `-l` flag, doing so tells `ls` to use a long listing format.

```
~/wargames/overthewire/bandit$ ssh bandit3@bandit.labs.overthewire.org -p 2220
bandit3@bandit~$ ls
inhere
bandit3@bandit~$ cd inhere/
bandit3@bandit~/inhere$ ls -lA
total 12
-rw-r----- 1 bandit4 bandit3   33 Oct 16  2018 .hidden
bandit3@bandit~/inhere$ cat .hidden
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
bandit3@bandit~/inhere$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 4 → Level 5

**Level Goal:**
*The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.*

Here we can see a bunch of files. We could `cat` these one by one or us the `find` command to filter for something, but I'm lazy.

```
~/wargames/overthewire/bandit$ ssh bandit4@bandit.labs.overthewire.org -p 2220

bandit4@bandit~$ ls -lA
total 24
-rw-r--r--  1 root root  220 May 15  2017 .bash_logout
-rw-r--r--  1 root root 3526 May 15  2017 .bashrc
drwxr-xr-x  2 root root 4096 Oct 16  2018 inhere
-rw-r--r--  1 root root  675 May 15  2017 .profile
bandit4@bandit~$ cd inhere/
bandit4@bandit~/inhere$ ls -lA
total 48
-rw-r----- 1 bandit5 bandit4   33 Oct 16  2018 -file00
-rw-r----- 1 bandit5 bandit4   33 Oct 16  2018 -file01
-rw-r----- 1 bandit5 bandit4   33 Oct 16  2018 -file02
-rw-r----- 1 bandit5 bandit4   33 Oct 16  2018 -file03
-rw-r----- 1 bandit5 bandit4   33 Oct 16  2018 -file04
-rw-r----- 1 bandit5 bandit4   33 Oct 16  2018 -file05
-rw-r----- 1 bandit5 bandit4   33 Oct 16  2018 -file06
-rw-r----- 1 bandit5 bandit4   33 Oct 16  2018 -file07
-rw-r----- 1 bandit5 bandit4   33 Oct 16  2018 -file08
-rw-r----- 1 bandit5 bandit4   33 Oct 16  2018 -file09
```

We can simply use the wildcard character `*` to select all files that match `./-file0*`. If we simply run the `cat` command like this `cat ./-file0*` will probably get a bunch of gooballygoop in our terminal. To avoid that we can use `strings` and the pipeline `|`. We `cat` all the `-file`'s and pass the output of all these files to `strings` . `strings` will filter out all the gooballygoop and only return human-readable text.

```
bandit4@bandit~/inhere$ cat ./-file0* | strings
w$N?c
ZP*E
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
bandit4@bandit~/inhere$ logout
Connection to bandit.labs.overthewire.org closed.
```
