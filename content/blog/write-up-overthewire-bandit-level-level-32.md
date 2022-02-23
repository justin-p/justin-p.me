---
author: "Justin Perdok"
categories: ["Bandit", "OverTheWire", "ctf", "Write-Up", "wargames"]
date: 2019-09-12T16:20:53Z
description: ""
draft: false
slug: "write-up-overthewire-bandit-level-level-32"
summary: "$ENV ME UP SCOTTY."
tags: ["Bandit", "OverTheWire", "ctf", "Write-Up", "wargames"]
title: "Write-up: OverTheWire - Bandit Level Level 32 → Level 33"
---

### Bandit Level 32 → Level 33

**Level Goal:**
*After all this git stuff its time for another escape. Good luck!*

This level requires is to log in with `ssh` with bandit32. That means its time to say goodbye to our MoreVimShell.

When we connect we are created with by someone seems to have had a field day with caps lock enabled. It seems we have a some sort of shell where every command we type is converted to uppercase. We are able to call some environment variables though.

```
ssh bandit32@bandit.labs.overthewire.org -p 2220

WELCOME TO THE UPPERCASE SHELL
>>
>> ls
sh: 1: LS: not found
>> echo
sh: 1: ECHO: not found
>> printenv
sh: 1: PRINTENV: not found
>> $SHELL
WELCOME TO THE UPPERCASE SHELL
>> $PATH
sh: 1: /usr/local/bin:/usr/bin:/bin:/usr/games: not found
>> $BASH
```

I then decided to try some random [environment variables](https://www.cyberciti.biz/faq/linux-list-all-environment-variables-env-command/).

```
>> $BASH_VERSION
>> $HOSTNAME
>> $CDPATH
>> $HISTFILE
>> $HISTFILESIZE
>> $HISTSIZE
>> $HOME
sh: 1: /home/bandit32: Permission denied
>> $IFS
>> $LANG
sh: 1: en_US.UTF-8: not found
>> $PATH
sh: 1: /usr/local/bin:/usr/bin:/bin:/usr/games: not found
>> $PS1
sh: 1: $: not found
>> $TMOUT
>> $TERM
sh: 1: xterm-256color: not found
>> $DISPLAY
>> $EDITOR
```

Since this yielded no results I to read over the `bash` man page where I stumbled on `Special Parameters`.

Where `$0` looked someone what promising. This could contain something like `/bin/sh`.

> ($0) Expands to the name of the shell or shell script. This is set at shell initialization. If Bash is invoked with a file of commands (see Shell Scripts), $0 is set to the name of that file. If Bash is started with the -c option (see Invoking Bash), then $0 is set to the first argument after the string to be executed, if one is present. Otherwise, it is set to the filename used to invoke Bash, as given by argument zero.

Which seemed to be the case. This gave me a `/bin/sh` shell where I spawned `/bin/bash` and proceeded to `cat` the password file.

```
>> $*
>> $@
>> $#
sh: 1: 0: not found
>> $?
sh: 1: 0: not found
>> $-
>> $$
sh: 1: 6806: not found
>> $!
>> $0
$
$ /bin/bash
bandit33@bandit:~$
bandit33@bandit:~$ cat /etc/bandit_pass/bandit33
c9c3199ddf4121b10cf581a98d51caee
```
