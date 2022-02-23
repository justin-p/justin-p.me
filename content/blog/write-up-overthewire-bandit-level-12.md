---
author: "Justin Perdok"
categories:
  [
    "Bandit",
    "ctf",
    "OverTheWire",
    "wargames",
    "Write-Up",
    "file",
    "tar",
    "zcat",
    "bzip2",
  ]
date: 2019-09-11T22:55:39Z
description: ""
draft: false
slug: "write-up-overthewire-bandit-level-12"
summary: "Decompress dat hex."
tags:
  [
    "Bandit",
    "ctf",
    "OverTheWire",
    "wargames",
    "Write-Up",
    "file",
    "tar",
    "zcat",
    "bzip2",
  ]
title: "Write-up: OverTheWire - Bandit Level 12 → level 13"
---

### Bandit Level 12 → Level 13

**Level Goal:**
*The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)*

So lets see what we have. As the goal explained a data.txt which is a hexdump of compressed data.

```
~/wargames/overthewire/bandit$ ssh bandit12@bandit.labs.overthewire.org -p 2220

bandit12@bandit~$ ls -la
total 24
drwxr-xr-x  2 root     root     4096 Oct 16  2018 .
drwxr-xr-x 41 root     root     4096 Oct 16  2018 ..
-rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
-rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
-rw-r-----  1 bandit13 bandit12 2581 Oct 16  2018 data.txt
-rw-r--r--  1 root     root      675 May 15  2017 .profile
bandit12@bandit~$ cat data.txt
00000000: 1f8b 0808 d7d2 c55b 0203 6461 7461 322e  .......[..data2.
00000010: 6269 6e00 013c 02c3 fd42 5a68 3931 4159  bin..<...BZh91AY
00000020: 2653 591d aae5 9800 001b ffff de7f 7fff  &amp;SY.............
00000030: bfb7 dfcf 9fff febf f5ad efbf bbdf 7fdb  ................
00000040: f2fd ffdf effa 7fff fbd7 bdff b001 398c  ..............9.
00000050: 1006 8000 0000 0d06 9900 0000 6834 000d  ............h4..
00000060: 01a1 a000 007a 8000 0d00 0006 9a00 d034  .....z.........4
00000070: 0d1a 3234 68d1 e536 a6d4 4000 341a 6200  ..24h..6..@.4.b.
00000080: 0069 a000 0000 0000 d003 d200 681a 0d00  .i..........h...
00000090: 0001 b51a 1a0c 201e a000 6d46 8068 069a  ...... ...mF.h..
000000a0: 6834 340c a7a8 3406 4000 0680 0001 ea06  h44...4.@.......
000000b0: 8190 03f5 4032 1a00 0343 4068 0000 0686  ....@2...C@h....
000000c0: 8000 0320 00d0 0d00 0610 0014 1844 0308  ... .........D..
000000d0: 04e1 c542 9ab8 2c30 f1be 0b93 763b fb13  ...B..,0....v;..
000000e0: 50c4 c101 e008 3b7a 92a7 9eba 8a73 8d21  P.....;z.....s.!
000000f0: 9219 9c17 052b fb66 a2c2 fccc 9719 b330  .....+.f.......0
00000100: 6068 8c65 e504 5ec0 ae02 fa6d 16bc 904b  `h.e..^....m...K
00000110: ba6c f692 356e c02b 0374 c394 6859 f5bb  .l..5n.+.t..hY..
00000120: 0f9f 528e 4272 22bb 103c 2848 d8aa 2409  ..R.Br"..<(H..$.
00000130: 24d0 d4c8 4b42 7388 ce25 6c1a 7ec1 5f17  $...KBs..%l.~._.
00000140: cc18 ddbf edc1 e3a4 67f1 7a4d 8277 c823  ........g.zM.w.#
00000150: 0450 2232 40e0 07f1 ca16 c6d6 ef0d ecc9  .P"2@...........
00000160: 8bc0 5e2d 4b12 8586 088e 8ca0 e67d a55c  ..^-K........}.\
00000170: 2ca0 18c7 bfb7 7d45 9346 ea5f 2172 01e4  ,.....}E.F._!r..
00000180: 5598 673f 45af 69b7 a739 7814 8706 04ed  U.g?E.i..9x.....
00000190: 5442 1240 0796 6cc8 b2f6 1ef9 8d13 421d  TB.@..l.......B.
000001a0: 461f 2e68 4d91 5343 34b5 56e7 46d0 0a0a  F..hM.SC4.V.F...
000001b0: 72b7 d873 71d9 6f09 c326 402d dbc0 7cef  r..sq.o..&amp;@-..|.
000001c0: 53b1 df60 9ec7 f318 00df 3907 2e85 d85b  S..`......9....[
000001d0: 6a1a e105 0207 c580 e31d 82d5 8646 183c  j............F.<
000001e0: 6a04 4911 101a 5427 087c 1f94 47a2 270d  j.I...T'.|..G.'.
000001f0: ad12 fc5c 9ad2 5714 514f 34ba 701d fb69  ...\..W.QO4.p..i
00000200: 8eed 0183 e2a1 53ea 2300 26bb bd2f 13df  ......S.#.&amp;../..
00000210: b703 08a3 2309 e43c 44bf 75d4 905e 5f96  ....#..<D.u..^_.
00000220: 481b 362e e82d 9093 7741 740c e65b c7f1  H.6..-..wAt..[..
00000230: 5550 f247 9043 5097 d626 3a16 da32 c213  UP.G.CP..&amp;:..2..
00000240: 2acd 298a 5c8a f0c1 b99f e2ee 48a7 0a12  *.).\.......H...
00000250: 03b5 5cb3 0037 cece 773c 0200 00         ..\..7..w<...
```

Lets create a folder in /tmp/ where we can work and copy over this file.

```
bandit12@bandit~$ mkdir /tmp/esc
bandit12@bandit~$ cd /tmp/esc
bandit12@bandit/tmp/esc$
bandit12@bandit/tmp/esc$ cp ~/data.txt data.hex
bandit12@bandit/tmp/esc$ ls
data.hex
```

First thing first, lets return this hexdump back to a file with `xxd`. `xxd` has a a revert flag `-r`. With this you can restore a hexdump back to its original binary form.

```
bandit12@bandit/tmp/esc$ xxd -r data.hex >> reversedhex.out
```

Now lets check what this file is with the `file` command.

```bandit12@bandit/tmp/esc$ file reversedhex.out
reversedhex.out: gzip compressed data, was "data2.bin", last modified: Tue Oct 16 12:00:23 2018, max compression, from Unix
```

Okay its a `gzip` compressed file. We could use `gunzip` to decompress, but it tents to be iffy about file names, so lets use `zcat` instead. `zcat` will decompress files whether they have a .gz suffix or not.

```
bandit12@bandit/tmp/esc$ zcat reversedhex.out > zcat_out.1
bandit12@bandit/tmp/esc$ file zcat_out.1
zcat_out.1: bzip2 compressed data, block size = 900k
```

So now we have a bzip2 file. No problem lets use `bzip -d` . Then we get a `gzip` again, lets `zcat` again. Now we have a `tar` archive. And another `tar`.

```
bandit12@bandit/tmp/esc$ bzip2 -d zcat_out.1
bzip2: Can't guess original name for zcat_out.1 -- using zcat_out.1.out
bandit12@bandit/tmp/esc$ file zcat_out.1.out
zcat_out.1.out: gzip compressed data, was "data4.bin", last modified: Tue Oct 16 12:00:23 2018, max compression, from Unix
bandit12@bandit/tmp/esc$ zcat zcat_out.1.out > zcat_out.2
bandit12@bandit/tmp/esc$ file zcat_out.2
zcat_out.2: POSIX tar archive (GNU)
bandit12@bandit/tmp/esc$ tar xvf zcat_out.2
data5.bin
bandit12@bandit/tmp/esc$ file data5.bin
data5.bin: POSIX tar archive (GNU)
```

I'm beginning to sense a pattern here. Let's stop doing this by hand and script it.

Lets create a file named `unpackhex.sh` and make it executable. Then copy over the original file to data.hex and reverse it back to a file, then open vim and create our script.

```
bandit12@bandit/tmp/esc$ touch unpackhex.sh
bandit12@bandit/tmp/esc$ chmod +x unpackhex.sh
bandit12@bandit/tmp/esc$ cp ~/data.txt data.hex
bandit12@bandit/tmp/esc$ xxd -r data.hex >> reversed.hex
bandit12@bandit/tmp/esc$ vim unpackhex.sh
```

Lets start with a scaffold. We known we want some function to check what file type the current file is and some function that we call in a loop that will decompresses the files.

```
#bin/bash
# function to get filetype using file and cut
function get_file_type () {
    # get file type
}
# function to match filetypes to correct command
function decompress () {
    # check file type
    # if statements to use correct decompress command
    # if filetype is ASCCI stop the loop.
}
LOOP=1
until [ $LOOP = 0 ]
do
        # call decompress function
done
```

For the `get_file_type` function we can use `file` and `cut`. We can use `cut` to remove sections from the line that `file` returns. If we look at the string that `file` returns we see that the file type is encased with spaces.

```
reversedhex.out: gzip compressed data, was "data2.bin", last modified: Tue Oct 16 12:00:23 2018, max compression, from Unix
```

If we simply tell `cut` to use spaces as delimiters with the `-d` flag we should be good. We only need to select the correct field with the `-f` flag, which is `2`.

Our function now looks like this. Now lets focus on the decompression function.

```
function get_file_type () {
        file $1 | cut -d " " -f 2
}
```

We first call the `get_file_type` function with the input ( `$1` ) of the decompress function and store that in a variable called `FILETYPE`. We will then create a nice `echo` to give some info about the current file type being matched to.

```
# function to match filetypes to correct command
function decompress () {
        FILETYPE=$(get_file_type $1)
        echo "[+] $1 matched to $FILETYPE filetype"
}
```

Now lets create some `if-statements`. We know that there are 4 possible file types `gzip` `bzip2` `tar` and probally `ASCII` once we hit the password file. Lets checks for each filetype and and finaly `else` as a catch in the case that we hit an unknown file type. (We also add a sleep in there, just in case something weird happens in our code logic.)

```
# function to match filetypes to correct command
function decompress () {
        FILETYPE=$(get_file_type $1)
        echo "[+] $1 matched to $FILETYPE filetype"
        if [ "$FILETYPE" = "gzip" ]; then
                #
        elif [ "$FILETYPE" = "bzip2" ]; then
                #E
        elif [ "$FILETYPE" = "POSIX" ]; then
                #
        elif [ "$FILETYPE" = "ASCII" ]; then
                #
        else
                echo "$FILETYPE is unknown"
        fi
        sleep 1
}
```

Now that we have our `if-statements` its a simple case of using the right command to decompress the filetype, but before we do that lets create another `if-statement` and variable called `FILENAME`.

Our decompression commands need a filename to decompress to. We want to store this filename in a variable so we know what we should check in the next instance on the loop. We could create a function that checks for newly created files, but this is way easier imo.

However, we don't want to overwrite the value in `FILENAME` once we hit the `ASCII` file type. In this case we simply want to keep `FILENAME` set to the same value of the previous loop. We then simply call `cat` the using the `FILENAME` variable. To check for this we can do a `does not equal` check on `ASCCI` (`!=`).

To generate a random filename we use `$RANDOM`. `$RANDOM` is an internal Bash function that returns integer in the range 0 - 32767. There is a slight chance that we get a duplicate file name, but lets risk it.

`zcat` and `bzcat` both work nicely with redirection the output to `$FILENAME`. `tar` how ever does not. To still store the `FILENAME` we simply store the output of `tar` in `FILENAME` like this `FILENAME=$(tar xvf $1)` .

Our `decompress` function now looks like this. Now that we have that setup we can focus on the loop.

```
# function to match filetypes to correct command
function decompress () {
        FILETYPE=$(get_file_type $1)
        if [ "$FILETYPE" != "ASCII" ]; then
                FILENAME="out_$RANDOM"
        fi
        echo "[+] $1 matched to $FILETYPE filetype"
        if [ "$FILETYPE" = "gzip" ]; then
                zcat $1 > $FILENAME
        elif [ "$FILETYPE" = "bzip2" ]; then
                bzcat $1 > $FILENAME
        elif [ "$FILETYPE" = "POSIX" ]; then
                FILENAME=$(tar xvf $1)
        elif [ "$FILETYPE" = "ASCII" ]; then
                cat $FILENAME
        else
                echo "$FILETYPE is unknown"
        fi
        sleep 1
}
```

We setup a variable called `LOOP` with the value of `1`. We then create a `until` loop and tell it to run as long `LOOP` does not have the value of `0`. Then we call our `decompress` function.

```
LOOP=1
until [ $LOOP = 0 ]
do
        decompress $FILENAME
done
```

To stop the `until` loop once we hit the `ASCII` file type we update our `decompress` function. Once the `FILETYPE` matches `ASCII` and after we used `cat` to output the file we set the `LOOP` variable to `0`.

```
# function to match filetypes to correct command
function decompress () {
        FILETYPE=$(get_file_type $1)
        if [ "$FILETYPE" != "ASCII" ]; then
                FILENAME="out_$RANDOM"
        fi
        echo "[+] $1 matched to $FILETYPE filetype"
        if [ "$FILETYPE" = "gzip" ]; then
                zcat $1 > $FILENAME
        elif [ "$FILETYPE" = "bzip2" ]; then
                bzcat $1 > $FILENAME
        elif [ "$FILETYPE" = "POSIX" ]; then
                FILENAME=$(tar xvf $1)
        elif [ "$FILETYPE" = "ASCII" ]; then
                cat $FILENAME
                LOOP=0
        else
                echo "$FILETYPE is unknown"
        fi
        sleep 1
}
```

The only thing left to do is to set the first instance `FILENAME` so the `decompress` function knows where to start, so lets just do that with `read` so we can supply this on the commandline.

```
echo "[?] What file should we decompress? "
read FILENAME
LOOP=1
until [ $LOOP = 0 ]
do
        decompress $FILENAME
done
```

Here is our final script.

```
#bin/bash
# function to get filetype using file and cut
function get_file_type () {
        file $1 | cut -d " " -f 2
}
# function to match filetypes to correct command
function decompress () {
        FILETYPE=$(get_file_type $1)
        echo "[+] $1 matched to $FILETYPE filetype"
        if [ "$FILETYPE" != "ASCII" ]; then
                FILENAME="out_$RANDOM"
        fi
        if [ "$FILETYPE" = "gzip" ]; then
                zcat $1 > $FILENAME
        elif [ "$FILETYPE" = "bzip2" ]; then
                bzcat $1 > $FILENAME
        elif [ "$FILETYPE" = "POSIX" ]; then
                FILENAME=$(tar xvf $1)
        elif [ "$FILETYPE" = "ASCII" ]; then
                cat $FILENAME
                LOOP=0
        else
                echo "$FILETYPE is unknown"
        fi
        sleep 1
}
echo "[?] What file should we decompress? "
read FILENAME
LOOP=1
until [ $LOOP = 0 ]
do
        decompress $FILENAME
done
```

And here it is in action 🔮

```
bandit12@bandit/tmp/esc$ ./unpackhex.sh
[?] What file should we decompress?
reversed.hex
[+] reversed.hex matched to gzip filetype
[+] out_21267 matched to bzip2 filetype
[+] out_14023 matched to gzip filetype
[+] out_30556 matched to POSIX filetype
[+] data5.bin matched to POSIX filetype
[+] data6.bin matched to bzip2 filetype
[+] out_30902 matched to POSIX filetype
[+] data8.bin matched to gzip filetype
[+] out_14824 matched to ASCII filetype
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
bandit12@bandit/tmp/esc$ logout
Connection to bandit.labs.overthewire.org closed.
```
