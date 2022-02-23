---
author: "Justin Perdok"
categories:
  [
    "ctf",
    "OverTheWire",
    "wargames",
    "Write-Up",
    "grep",
    "sort",
    "tr",
    "base64",
    "Bandit",
    "strings",
  ]
date: 2019-09-11T22:04:57Z
description: ""
draft: false
slug: "write-up-overthewire-bandit-level-7-level-12"
summary: "grep, sort, decode and maybe some salad on the side."
tags:
  [
    "ctf",
    "OverTheWire",
    "wargames",
    "Write-Up",
    "grep",
    "sort",
    "tr",
    "base64",
    "Bandit",
    "strings",
  ]
title: "Write-up: OverTheWire - Bandit Level 7 → Level 12"
---

### Bandit Level 7 → Level 8

**Level Goal:**
*The password for the next level is stored in the file data.txt next to the word millionth*

Lets get grep'ing.

`grep` searches the supplied files (or standard input if no files arenamed) for lines containing a match to the given pattern.

```
~/wargames/overthewire/bandit$ ssh bandit7@bandit.labs.overthewire.org -p 2220

bandit7@bandit~$ ls -lA
total 4108
-rw-r--r--  1 root    root        220 May 15  2017 .bash_logout
-rw-r--r--  1 root    root       3526 May 15  2017 .bashrc
-rw-r-----  1 bandit8 bandit7 4184396 Oct 16  2018 data.txt
-rw-r--r--  1 root    root        675 May 15  2017 .profile
```

So we know the password is next to the word `millionth`, so lets `grep` that.

```
bandit7@bandit~$ grep millionth data.txt
millionth       cvX2JJa4CFALtqS87jk27qwqGhBM9plV
bandit7@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 8 → Level 9

**Level Goal:**
*The password for the next level is stored in the file data.txt and is the only line of text that occurs only once
*

So we know there's a file called `data.txt` that has 1 unique line. Lets use `sort` and `uniq` to find it.

```
~/wargames/overthewire/bandit$ ssh bandit8@bandit.labs.overthewire.org -p 2220

bandit8@bandit~$ ls -la
total 56
drwxr-xr-x  2 root    root     4096 Oct 16  2018 .
drwxr-xr-x 41 root    root     4096 Oct 16  2018 ..
-rw-r--r--  1 root    root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root    root     3526 May 15  2017 .bashrc
-rw-r-----  1 bandit9 bandit8 33033 Oct 16  2018 data.txt
-rw-r--r--  1 root    root      675 May 15  2017 .profile
```

`sort` sorts lines of text files. `uniq` can reports or omit repeated lines.

Lets use `sort` to sort `data.txt` and then pipe `|` it to `uniq` with the `-u` flag. The `-u` ensures that only unique lines are printed.

```
bandit8@bandit~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
bandit8@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 9 → Level 10

**Level Goal:**
*The password for the next level is stored in the file data.txt in one of the few human-readable strings, beginning with several ‘=’ characters.
*

Lets get grep'ing again. Lets filter out only `human-readable` text with `strings` and pipe `|` that to `grep`. Our `grep` filter is set to `==`.

```
~/wargames/overthewire/bandit$ ssh bandit9@bandit.labs.overthewire.org -p 2220

bandit9@bandit~$ ls -la
total 40
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r-----  1 bandit10 bandit9 19379 Oct 16  2018 data.txt
-rw-r--r--  1 root     root      675 May 15  2017 .profile
bandit9@bandit~$ strings data.txt | grep '=='
2========== the
========== password
========== isa
========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
bandit9@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 10 → Level 11

**Level Goal:**
*The password for the next level is stored in the file data.txt, which contains base64 encoded data
*

Where we find some `base64` encoded data. But what is `base64` encoding ? `base64` encoding is a process of converting `binary` data to an `ASCII` strings. The binary data is converted into a 6-bit character representation. `base64` is usually used in situations where binary data, such as images or video, needs to be transmitted over systems that are designed to transmit data in a plain-text format. However, you can also find being used with `SMTP` `AUTH LOGIN` or `HTML` `Basic Authentication`.

To solve this we can `cat` the file and pipe `|` this to `base64` with the `-d` flag. The `-d` flag will decode this back to `plaintext`.

```
~/wargames/overthewire/bandit$ ssh bandit10@bandit.labs.overthewire.org -p 2220

bandit10@bandit~$ ls -la
total 24
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r-----  1 bandit11 bandit10   69 Oct 16  2018 data.txt
-rw-r--r--  1 root     root      675 May 15  2017 .profile
bandit10@bandit~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg==
bandit10@bandit~$ cat data.txt | base64 -d
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
bandit10@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 11 → Level 12

**Level Goal:**
*The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
*

Aaahh, good ol' `ROT13`, also known as the `caesar cipher` . I still remember my first time trying to decode a password encoded with `ROT26` for the Certified Secure IRC.

`ROT` replaces the current letter by `X` defined in `ROT`. `ROT13` replaces the current letter with the 13th letter after it in the alphabet.

For example `a` becomes `n`, `b` becomes `o` and `c` becomes `p`.

We can decode this back to readable text using `tr`.

Here we use `tr` to replace `N to Z and A to M` with `A to Z` (both lower and uppercase).

```
~/wargames/overthewire/bandit$ ssh bandit11@bandit.labs.overthewire.org -p 2220

bandit11@bandit~$ ls -la
total 24
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r-----  1 bandit12 bandit11   49 Oct 16  2018 data.txt
-rw-r--r--  1 root     root      675 May 15  2017 .profile
bandit11@bandit~$ cat data.txt
Gur cnffjbeq vf 5Gr8L4qetPEsPk8htqjhRK8XSP6x2RHh
bandit11@bandit~$ cat data.txt |  tr '[A-Za-z]' '[N-ZA-Mn-za-m]'
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
bandit11@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```
