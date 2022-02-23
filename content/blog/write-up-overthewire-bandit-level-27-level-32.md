---
author: "Justin Perdok"
categories: ["git", "Bandit", "OverTheWire", "wargames", "Write-Up", "ctf"]
date: 2019-09-12T15:14:03Z
description: ""
draft: false
slug: "write-up-overthewire-bandit-level-27-level-32"
summary: "🤖 DaftPunk.git: clone it, fetch it, merge it, push it; pull it, log it, cherry-pick it"
tags: ["git", "Bandit", "OverTheWire", "wargames", "Write-Up", "ctf"]
title: "Write-up: OverTheWire - Bandit Level 27 → Level 32"
---

Bandit level 27 → level 32 will use our MoreVimShell created in Bandit level 26 → level 27.

### Bandit Level 27 → Level 28

**Level Goal:**
*There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo. The password for the user bandit27-git is the same as for the user bandit27.*

It seems we now move on to some `git`. If you have no idea what `git` is, give [this a read.](https://rogerdudler.github.io/git-guide/)

Lets setup a workspace where we call pull the `git` repo to.

```
bandit26@bandit:$ mkdir /tmp/giv_git
bandit26@bandit:$ cd /tmp/giv_git
bandit26@bandit:/tmp/giv_pass$
bandit26@bandit:/tmp/giv_git$ git clone ssh://bandit27-git@localhost/home/bandit27-git/repo.git
Cloning into 'repo'...
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit26/.ssh/known_hosts).


bandit27-git@localhost's password:
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.
bandit26@bandit:/tmp/giv_git$ ls
repo
```

Lets `cd` into the cloned folder and `cat` whats inside.

```
bandit26@bandit:/tmp/giv_git$ cd repo/
bandit26@bandit:/tmp/giv_git/repo$ ls
README
bandit26@bandit:/tmp/giv_git/repo$ cat README
The password to the next level is: 0ef186ac70e04ea33b4c1853d2526fa2
```

### Bandit Level 28 → Level 29

**Level Goal:**
*There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo. The password for the user bandit28-git is the same as for the user bandit28.
*

Lets remove the old repo and clone the new one and `cd` into it.

```
bandit26@bandit:/tmp/giv_git/repo$ cd ..
bandit26@bandit:/tmp/giv_git$ rm repo/ -rf
bandit26@bandit:/tmp/giv_git$ git clone ssh://bandit28-git@localhost/home/bandit28-git/repo.git
Cloning into 'repo'...
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit26/.ssh/known_hosts).


bandit28-git@localhost's password:
remote: Counting objects: 9, done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 9 (delta 2), reused 0 (delta 0)
Receiving objects: 100% (9/9), done.
Resolving deltas: 100% (2/2), done.
bandit26@bandit:/tmp/giv_git$ ls
repo
bandit26@bandit:/tmp/giv_git$ cd repo/
bandit26@bandit:/tmp/giv_git/repo$ ls -la
total 16
drwxr-sr-x 3 bandit26 root 4096 Sep 11 17:11 .
drwxr-sr-x 3 bandit26 root 4096 Sep 11 17:11 ..
drwxr-sr-x 8 bandit26 root 4096 Sep 11 17:11 .git
-rw-r--r-- 1 bandit26 root  111 Sep 11 17:11 README.md
```

Since there probably won't be a password in the README.md this time lets look at the `git logs` .

Here we see some previous commits with a reference to a info leak.

```
bandit26@bandit:/tmp/giv_git/repo$ git log
commit 073c27c130e6ee407e12faad1dd3848a110c4f95
Author: Morla Porla <morla@overthewire.org>
Date:   Tue Oct 16 14:00:39 2018 +0200

    fix info leak

commit 186a1038cc54d1358d42d468cdc8e3cc28a93fcb
Author: Morla Porla <morla@overthewire.org>
Date:   Tue Oct 16 14:00:39 2018 +0200

    add missing data

commit b67405defc6ef44210c53345fc953e6a21338cc7
Author: Ben Dover <noone@overthewire.org>
Date:   Tue Oct 16 14:00:39 2018 +0200

    initial commit of README.md
```

Lets git checkout that commit and `cat` the README.md file.

```
bandit26@bandit:/tmp/giv_git/repo$ git checkout 186a1038cc54d1358d42d468cdc8e3cc28a93fcb
Note: checking out '186a1038cc54d1358d42d468cdc8e3cc28a93fcb'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at 186a103... add missing data
bandit26@bandit:/tmp/giv_git/repo$ cat README.md

##Bandit Notes

Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: bbc96594b4e001778eee9975372716b2
```

### Bandit Level 29 → Level 30

**Level Goal:**
*There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo. The password for the user bandit29-git is the same as for the user bandit29
*

Lets remove the old repo and clone the new one and `cd` into it.

```
bandit26@bandit:/tmp/giv_git/repo$ cd ..
bandit26@bandit:/tmp/giv_git$ rm -rf repo/
bandit26@bandit:/tmp/giv_git$
bandit26@bandit:/tmp/giv_git$ git clone  ssh://bandit29-git@localhost/home/bandit29-git/repo
Cloning into 'repo'...
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit26/.ssh/known_hosts).


bandit29-git@localhost's password:
remote: Counting objects: 16, done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 16 (delta 2), reused 0 (delta 0)
Receiving objects: 100% (16/16), done.
Resolving deltas: 100% (2/2), done.
bandit26@bandit:/tmp/giv_git$ ls -la
total 2092
drwxr-sr-x     3 bandit26 root    4096 Sep 11 17:15 .
drwxrws-wt 18980 root     root 2129920 Sep 11 17:15 ..
drwxr-sr-x     3 bandit26 root    4096 Sep 11 17:15 repo
bandit26@bandit:/tmp/giv_git$ cd repo/
bandit26@bandit:/tmp/giv_git/repo$ ls -la
total 16
drwxr-sr-x 3 bandit26 root 4096 Sep 11 17:15 .
drwxr-sr-x 3 bandit26 root 4096 Sep 11 17:15 ..
drwxr-sr-x 8 bandit26 root 4096 Sep 11 17:15 .git
-rw-r--r-- 1 bandit26 root  131 Sep 11 17:15 README.md
```

If we `cat` the README.md there is a reference to 'passwords not being allowed in production'. I'd say passwords should not be allowed in version control software, but what do I know.

```
bandit26@bandit:/tmp/giv_git/repo$ cat README.md

# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```

Anyhow lets see if there are some other branches.

```
bandit26@bandit:/tmp/giv_git/repo$ git branch -a
* master
  remotes/origin/HEAD-> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

Lets `git checkout` that `dev` branch and cat the README.md.

```
bandit26@bandit:/tmp/giv_git/repo$ git checkout dev
Branch dev set up to track remote branch dev from origin.
Switched to a new branch 'dev'
bandit26@bandit:/tmp/giv_git/repo$ cat README.md

# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: 5b90576bedb2cc04c86a9e924ce42faf
```

### Bandit Level 30 → Level 31

**Level Goal:**
*There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo. The password for the user bandit30-git is the same as for the user bandit30.
*

Lets remove the old repo and clone the new one and `cd` into it.

```
bandit26@bandit:/tmp/giv_git/repo$ cd ..
bandit26@bandit:/tmp/giv_git$ rm -rf repo/
bandit26@bandit:/tmp/giv_git$ git clone ssh://bandit30-git@localhost/home/bandit30-git/repo
Cloning into 'repo'...
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit26/.ssh/known_hosts).


bandit30-git@localhost's password:
remote: Counting objects: 4, done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (4/4), done.
bandit26@bandit:/tmp/giv_git$
bandit26@bandit:/tmp/giv_git$ ls
repo
bandit26@bandit:/tmp/giv_git$ cd repo/
bandit26@bandit:/tmp/giv_git/repo$ ls -la
total 16
drwxr-sr-x 3 bandit26 root 4096 Sep 11 17:19 .
drwxr-sr-x 3 bandit26 root 4096 Sep 11 17:18 ..
drwxr-sr-x 8 bandit26 root 4096 Sep 11 17:19 .git
-rw-r--r-- 1 bandit26 root   30 Sep 11 17:19 README.md
```

Its seems someone placed a 'empty' file here. If we check `git log` we only see 1 commit and there are no other branches. So whats left ? `tags`.

Lets check if there are some tags with `git tag -l` and `git show-ref --tags`.

```
bandit26@bandit:/tmp/giv_git/repo$ cat README.md
just an epmty file... muahaha
andit26@bandit:/tmp/giv_git/repo/.git$ git tag -l
secret
bandit26@bandit:/tmp/giv_git/repo/.git$ git show-ref --tags
f17132340e8ee6c159e0a4a6bc6f80e1da3b1aea refs/tags/secret
```

Okay so we found some `tag` , how do we show this ? Well with `git show`.

```
bandit26@bandit:/tmp/giv_git/repo$ git show secret
47e603bb428404d265f59c42920d81e5
```

### Bandit Level 31 → Level 32

**Level Goal:**
*There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo. The password for the user bandit31-git is the same as for the user bandit31.
*

Lets remove the old repo and clone the new one and cd into it.

```
bandit26@bandit:/tmp/giv_git/repo$ cd ..
bandit26@bandit:/tmp/giv_git$ rm -rf repo/
bandit26@bandit:/tmp/giv_git$ ssh://bandit31-git@localhost/home/bandit31-git/repo
bash: ssh://bandit31-git@localhost/home/bandit31-git/repo: No such file or directory
bandit26@bandit:/tmp/giv_git$ git clone ssh://bandit31-git@localhost/home/bandit31-git/repo
Cloning into 'repo'...
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit26/.ssh/known_hosts).


bandit31-git@localhost's password:
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (4/4), done.
bandit26@bandit:/tmp/giv_git$
bandit26@bandit:/tmp/giv_git$ ls
repo
bandit26@bandit:/tmp/giv_git$ cd repo/
bandit26@bandit:/tmp/giv_git/repo$ ls -la
total 24
drwxr-sr-x 3 bandit26 root 4096 Sep 11 17:37 .
drwxr-sr-x 3 bandit26 root 4096 Sep 11 17:36 ..
drwxr-sr-x 8 bandit26 root 4096 Sep 11 17:37 .git
-rw-r--r-- 1 bandit26 root    6 Sep 11 17:35 .gitignore
-rw-r--r-- 1 bandit26 root  147 Sep 11 17:35 README.md
```

First things first, lets look at that `.gitignore` file. It seems that someone has setup this `.gitignore` file to exclude all `txt` files.

```
bandit26@bandit:/tmp/giv_git/repo$ cat .gitignore
*.txt
```

Lets look at the README.md.

```
bandit26@bandit:/tmp/giv_git/repo$ cat README.md
This time your task is to push a file to the remote repository.

Details:
    File name: key.txt
    Content: 'May I come in?'
    Branch: master
```

It wants us to commit and push a file named `key.txt` to the remote repo. As we know from looking at the `.gitignore` file all `txt` files are ignored and unable to be pushed. So lets just create the `key.txt` file with the correct contents and remove the `.gitignore` file.

```
bandit26@bandit:/tmp/giv_git/repo$ vim key.txt
May I come in?
bandit26@bandit:/tmp/giv_git/repo$ rm .gitignore
```

Before we are able to make a `commit` we need to tell `git` who are are.

```
bandit26@bandit:/tmp/giv_git/repo$ git config user.email "they@you.me"
bandit26@bandit:/tmp/giv_git/repo$ git config user.name "they@you.me"
```

Now lets add the `key.txt` file, create a `commit` and `push` it.

```
bandit26@bandit:/tmp/giv_git/repo$ git add key.txt
bandit26@bandit:/tmp/giv_git/repo$ git commit -m "Added key.txt"
[master 091067d] Added key.txt
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt

bandit26@bandit:/tmp/giv_git/repo$ git push
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit26/.ssh/known_hosts).


bandit31-git@localhost's password:
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 317 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: ### Attempting to validate files... ####
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
remote: Well done! Here is the password  for the next level:
remote: 56a9bf19c63d650ce78e6ec0354ee45e
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
To ssh://localhost/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://bandit31-git@localhost/home/bandit31-git/repo'
```
