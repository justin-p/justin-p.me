---
author: "Justin Perdok"
categories:
  [
    "Bandit",
    "OverTheWire",
    "wargames",
    "Write-Up",
    "ssh-keys",
    "netcat",
    "ncat",
    "openssl",
    "s_client",
    "ctf",
  ]
date: 2019-09-12T10:46:47Z
description: ""
draft: false
slug: "write-up-overthewire-bandit-level-13-level-16"
summary: "Lets cat the net!"
tags:
  [
    "Bandit",
    "OverTheWire",
    "wargames",
    "Write-Up",
    "ssh-keys",
    "netcat",
    "ncat",
    "openssl",
    "s_client",
    "ctf",
  ]
title: "Write-up: OverTheWire - Bandit Level 13 → Level 16"
---

### Bandit Level 13 → Level 14

**Level Goal:**
*The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on*

Pretty straight forward. If you never worked with `SSH keys` before I'd recommend you watch this [video](https://www.youtube.com/watch?v=uaW9xW4LdoA).

So lets login and `cat` the private key.

```
~/wargames/overthewire/bandit$ ssh bandit13@bandit.labs.overthewire.org -p 2220

bandit13@bandit~$ ls
sshkey.private
bandit13@bandit~$ cat sshkey.private
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAxkkOE83W2cOT7IWhFc9aPaaQmQDdgzuXCv+ppZHa++buSkN+
gg0tcr7Fw8NLGa5+Uzec2rEg0WmeevB13AIoYp0MZyETq46t+jk9puNwZwIt9XgB
ZufGtZEwWbFWw/vVLNwOXBe4UWStGRWzgPpEeSv5Tb1VjLZIBdGphTIK22Amz6Zb
ThMsiMnyJafEwJ/T8PQO3myS91vUHEuoOMAzoUID4kN0MEZ3+XahyK0HJVq68KsV
ObefXG1vvA3GAJ29kxJaqvRfgYnqZryWN7w3CHjNU4c/2Jkp+n8L0SnxaNA+WYA7
jiPyTF0is8uzMlYQ4l1Lzh/8/MpvhCQF8r22dwIDAQABAoIBAQC6dWBjhyEOzjeA
J3j/RWmap9M5zfJ/wb2bfidNpwbB8rsJ4sZIDZQ7XuIh4LfygoAQSS+bBw3RXvzE
pvJt3SmU8hIDuLsCjL1VnBY5pY7Bju8g8aR/3FyjyNAqx/TLfzlLYfOu7i9Jet67
xAh0tONG/u8FB5I3LAI2Vp6OviwvdWeC4nOxCthldpuPKNLA8rmMMVRTKQ+7T2VS
nXmwYckKUcUgzoVSpiNZaS0zUDypdpy2+tRH3MQa5kqN1YKjvF8RC47woOYCktsD
o3FFpGNFec9Taa3Msy+DfQQhHKZFKIL3bJDONtmrVvtYK40/yeU4aZ/HA2DQzwhe
ol1AfiEhAoGBAOnVjosBkm7sblK+n4IEwPxs8sOmhPnTDUy5WGrpSCrXOmsVIBUf
laL3ZGLx3xCIwtCnEucB9DvN2HZkupc/h6hTKUYLqXuyLD8njTrbRhLgbC9QrKrS
M1F2fSTxVqPtZDlDMwjNR04xHA/fKh8bXXyTMqOHNJTHHNhbh3McdURjAoGBANkU
1hqfnw7+aXncJ9bjysr1ZWbqOE5Nd8AFgfwaKuGTTVX2NsUQnCMWdOp+wFak40JH
PKWkJNdBG+ex0H9JNQsTK3X5PBMAS8AfX0GrKeuwKWA6erytVTqjOfLYcdp5+z9s
8DtVCxDuVsM+i4X8UqIGOlvGbtKEVokHPFXP1q/dAoGAcHg5YX7WEehCgCYTzpO+
xysX8ScM2qS6xuZ3MqUWAxUWkh7NGZvhe0sGy9iOdANzwKw7mUUFViaCMR/t54W1
GC83sOs3D7n5Mj8x3NdO8xFit7dT9a245TvaoYQ7KgmqpSg/ScKCw4c3eiLava+J
3btnJeSIU+8ZXq9XjPRpKwUCgYA7z6LiOQKxNeXH3qHXcnHok855maUj5fJNpPbY
iDkyZ8ySF8GlcFsky8Yw6fWCqfG3zDrohJ5l9JmEsBh7SadkwsZhvecQcS9t4vby
9/8X4jS0P8ibfcKS4nBP+dT81kkkg5Z5MohXBORA7VWx+ACohcDEkprsQ+w32xeD
qT1EvQKBgQDKm8ws2ByvSUVs9GjTilCajFqLJ0eVYzRPaY6f++Gv/UVfAPV4c+S0
kAWpXbv5tbkkzbS0eaLPTKgLzavXtQoTtKwrjpolHKIHUz6Wu+n4abfAIRFubOdN
/+aLoRQ0yBDRbdXMsZN/jvY44eM+xRLdRVyMmdPtP8belRi2E2aEzA==
-----END RSA PRIVATE KEY-----
bandit13@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

Instead of using `ssh` and `cat` we also could have used `scp` or `sftp` to download all the files from the home directory.

```
~/wargames/overthewire/bandit$ mkdir bandit13/outfolder -p
~/wargames/overthewire/bandit$ scp -P 2220  bandit13@bandit.labs.overthewire.org:* $PWD/bandit13/outfolder
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit13@bandit.labs.overthewire.org's password:
sshkey.private                                                                                                                                         100% 1679     1.6KB/s   00:00
```

```
~/wargames/overthewire/bandit$ sftp -P 2220  bandit13@bandit.labs.overthewire.org:* $PWD/bandit13/outfolder
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

Connected to bandit.labs.overthewire.org.
Fetching /home/bandit13/sshkey.private to ~/wargames/overthewire/bandit/bandit13/outfolder/sshkey.private
/home/bandit13/sshkey.private                                                                                                                          100% 1679     1.6KB/s   00:00
```

Now lets setup our private key for bandit14 and login using the private key by setting the `-i` flag.

```
~/wargames/overthewire/bandit$ mkdir bandit14
~/wargames/overthewire/bandit$ vim bandit14/id_rsa

* paste private key content here *

~/wargames/overthewire/bandit$ chmod 0400 bandit14/id_rsa

~/wargames/overthewire/bandit$ ssh bandit14@bandit.labs.overthewire.org -p 2220 -i bandit14/id_rsa

bandit14@bandit~$ cat /etc/bandit_pass/bandit14
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
bandit14@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

### Bandit Level 14 → 15

**Level Goal:**
*The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.
*

Now where getting to the fun part. We somehow need do talk with `127.0.0.1:30000` and send over the password of bandit14. For this we can use netcat ( `nc` ). We tell netcat to connect to `localhost` on port `30000` and then paste the password of bandit14.

```
~/wargames/overthewire/bandit$ ssh bandit14@bandit.labs.overthewire.org -p 2220 -i bandit14/id_rsa

bandit14@bandit~$ nc 127.0.0.1 30000
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr

bandit14@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

We also could have used the pipeline `|` and `echo` to submit the password.

```
bandit14@bandit~$ echo 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e | nc localhost 30000
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr

bandit14@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

We also could have used `telnet` , `ncat` and even `/dev/tcp/` to acomplish this, but using `nc` seemed the most straight forward one.

### Bandit Level 15

**Level Goal:**
*The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.
*

Level 15 is basiclly the same as level 14, only that we need to submit the date over an ssl encrypted connection. Here we run into a limiation of `nc`, it does not support SSL/TLS. We could use its big brother `ncat` with the `--ssl` flag but i opted to use `openssl s_client` .

If we connect using `s_client` with the `--connect` flag we get a bunch of SSL/TLS info. This can be very usefull when troubleshooting something, but right now its just unneeded output, so lets hide that with the `--quite` flag.

```
~/wargames/overthewire/bandit$ ssh bandit15@bandit.labs.overthewire.org -p 2220

bandit15@bandit:~$ openssl s_client --connect 127.0.0.1:30001
CONNECTED(00000003)
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
---
Certificate chain
 0 s:/CN=localhost
   i:/CN=localhost
---
Server certificate
-----BEGIN CERTIFICATE-----
MIICBjCCAW+gAwIBAgIEfkeLojANBgkqhkiG9w0BAQUFADAUMRIwEAYDVQQDDAls
b2NhbGhvc3QwHhcNMTkwODAzMDc0OTMxWhcNMjAwODAyMDc0OTMxWjAUMRIwEAYD
VQQDDAlsb2NhbGhvc3QwgZ8wDQYJKoZIhvcNAQEBBQADgY0AMIGJAoGBAMDGwHmT
GntqHvPYiM0wm4Dsmhlmiywaj0CGZKW1Cx6ze9pH+iWXEvcnWga4Kfevqh0LJLeS
jmgE6hFRK9rTwq+q6UE0hADazxb7r8jpthnHwKyRGEtFmsFTv/JqJDk+V5cngA4Y
m4scTjF+r1Y7jQA5VkUPHy+eYoNoqRqGh7JhAgMBAAGjZTBjMBQGA1UdEQQNMAuC
CWxvY2FsaG9zdDBLBglghkgBhvhCAQ0EPhY8QXV0b21hdGljYWxseSBnZW5lcmF0
ZWQgYnkgTmNhdC4gU2VlIGh0dHBzOi8vbm1hcC5vcmcvbmNhdC8uMA0GCSqGSIb3
DQEBBQUAA4GBAEICbhntCy/wyg56HQpox3nt8YtTkr6g21P4akxso7m08u6FuyiY
t/8yd+iph6qlRDHQ+D8T4TcpflsV8YKPXIgMoJQtGkuVgqHeCfgBEJcx+T52F8aX
84l5d7tEr9XEuCPKIlf4/GL8wOQLww2a2+sjlSwa8S1XlkbC61JzPyS3
-----END CERTIFICATE-----
subject=/CN=localhost
issuer=/CN=localhost
---
No client certificate CA names sent
Peer signing digest: SHA512
Server Temp Key: X25519, 253 bits
---
SSL handshake has read 1019 bytes and written 269 bytes
Verification error: self signed certificate
---
New, TLSv1.2, Cipher is ECDHE-RSA-AES256-GCM-SHA384
Server public key is 1024 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES256-GCM-SHA384
    Session-ID: 56E09A068EFCBBA82B2857C0A466039818318F155FF7B803F42479FE2C282F96
    Session-ID-ctx:
    Master-Key: 3F48C060E258B3960B32611D84E4CEE0972369A0F49C144E6D5D350EBA53C33F1A5DDD4C6E832680E9EC2E25C07D7C0B
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    TLS session ticket lifetime hint: 7200 (seconds)
    TLS session ticket:
    0000 - 6a 32 a6 c7 27 26 f0 c8-b4 75 99 96 41 ed a8 13   j2..'&...u..A...
    0010 - ca fb 15 68 4f 3f e7 fa-d3 e0 1a e1 8c 68 41 8a   ...hO?.......hA.
    0020 - 2a 22 3d 08 06 e7 e9 07-5c 2d 2c 79 3d 0f 87 97   *"=.....\-,y=...
    0030 - 2d 41 11 9e 74 fc d3 4d-07 8d 1e da 50 75 38 05   -A..t..M....Pu8.
    0040 - 29 31 72 c9 12 dc d4 b8-da b9 8b 0f 2f a2 3b 67   )1r........./.;g
    0050 - 5f 77 68 8a dd 6e 53 25-5f 1c 9d 22 82 50 c7 5b   _wh..nS%_..".P.[
    0060 - 62 eb 32 d0 2d e6 10 a5-f6 07 87 77 f1 7c de 8d   b.2.-......w.|..
    0070 - 87 d0 eb 71 35 0b 45 a4-73 e3 23 94 01 a9 6b 4d   ...q5.E.s.#...kM
    0080 - c3 a7 b7 d6 bb 1d af 0c-86 54 f8 3f 1b 52 a6 e5   .........T.?.R..
    0090 - 3c fe 96 de 34 4b af 09-b5 37 e1 cb b8 2e 81 cf   <...4K...7......

    Start Time: 1568284761
    Timeout   : 7200 (sec)
    Verify return code: 18 (self signed certificate)
    Extended master secret: yes
```

Way better.

```
bandit15@bandit~$ openssl s_client --connect 127.0.0.1:30001 -quiet
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
```

So lets submit the password and see what happens.

```
bandit15@bandit~$ openssl s_client --connect 127.0.0.1:30001 -quiet
depth=0 CN = localhost
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = localhost
verify return:1
BfMYroe26WYalil77FoDi9qh59eK5xNr
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd

bandit15@bandit~$ logout
Connection to bandit.labs.overthewire.org closed.
```

Bingo👨‍💻
