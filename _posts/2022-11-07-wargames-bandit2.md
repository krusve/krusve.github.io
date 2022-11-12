---
title: "OverTheWire Wargames: Bandit Level 11-20"
date: 2022-11-07 20:13:00 +0100
categories: [Wargames, Bandit]
tags: [bandit, linux, overthewire, wargames, commandline]     # TAG names should always be lowercase
---


My Write-up of the infamous [Bandit](https://overthewire.org/wargames/bandit/) challenges, that "can help you to learn and practice security concepts in the form of fun-filled games."

# General
# General
<hr>

Usernames have the following format: bandit\<level>, so bandit0, bandit1 ...

To connect the following command is used:
```bash
$ ssh bandit<level>@bandit.labs.overthewire.org -p 2220
```
Then you will be asked to give the passwords, which you will find within each level.

Complex commands will be explained for each level, while the general commands can be found in a table at the end of the write-up.



## Level 11

<hr>

- Username: bandit11 <br>
- Password: 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM <br>
- Task: The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

```bash
bandit11@bandit:~$ cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
The password is jVNBBFSmzwKKOPØXbFXOoW8chDz5yVRv 
```

- tr 'A-Za-z' 'N-ZA-Mn-za-m'
  * "tr": translate or delete characters
  * "'A-Za-z' 'N-ZA-Mn-za-m'": Turn A to N, N to A, a to n ... (13 positions)

## Level 12

<hr>

- Username: bandit12 <br>
- Password: jVNBBFSmzwKKOPØXbFXOoW8chDz5yVRv <br>
- Task: The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)
  
### Create new directory and copy file there
```bash
bandit12@bandit:~$ mkdir /tmp/kruse3110
bandit12@bandit:~$ cp data.txt /tmp/kruse3110 
bandit12@bandit:~$ cd /tmp/kruse3110 
bandit12@bandit:/tmp/kruse3110$ ls 
data.txt
```

### Look at hex and extract data
```bash
bandit12abandit:/tmp/kruse3110$ head data .txt 
00000000: 1f8b 0808 0650 b45e 0203 6461 7461 322e  .....qQ.c..data2.
00000010: 6269 6e00 013d 02c2 fd42 5a68 3931 4159  bin..?...BZh91AY
00000020: 2653 598e 4f1c c800 001e 7fff fbf9 7fda  &SY.O...........
```
This tells us that we are dealing with a gzip file, as seen by the signature 1f8b.
But to use the file we first have to extract the data from the hexdump as written in the task:
```bash
bandit12@bandit:/tmp/kruse3110$ xxd -r hexdump_init hexdump_1
bandit12@bandit:/tmp/kruse3110$ head hexdump_1
P�^data2.bin=��BZh91AY&SY�O����ڞOv���}?��}��^����������ߣ��;����▒4��▒h�F�F��4▒LM
                                                                               @��z��FM��C�hF�C@�4@f��h
4hh�▒=C%�>X▒,�k���1��GY��                                                                              ��i�hPh�Mh
```

### Extract gzip file
As earlier seen by the signature, we are dealing with a gzip file and extract it as follows:
```bash
bandit12@bandit:/tmp/kruse3110$ cp hexdump_1 gzip_1.gz
bandit12@bandit:/tmp/kruse3110$ ls
data.txt gzip_1 hexdump_1 hexdump_init
bandit12@bandit:/tmp/kruse3110$ head gzip_1
P�^data2.bin=��BZh91AY&SY�O����ڞOv���}?��}��^����������ߣ��;����▒4��▒h�F�F��4▒LM
                                                                               @��z��FM��C�hF�C@�4@f��h
4hh�▒=C%�>X▒,�k���1��GY��                                                                              ��i�hPh�Mh
```
We are unable to read the gzip file but can find the file signature by using the xxd command to ouput the hex:
```bash
bandit12@bandit:/tmp/kruse3110$ xxd gzip_1
00000000: 425a 6839 3141 5926 5359 8e4f 1cc8 0000  BZh91AY&SY.O....
```
The file shows it's a bzip2 file (425a)

### Extract bzip2 file

```bash
bandit12@bandit:/tmp/kruse3110$ cp gzip_1.gz bzip_1.bz2
bandit12@bandit:/tmp/kruse3110$ bzip2 -d bzip_1.bz2
bandit12@bandit:/tmp/kruse3110$ ls
bzip_1 data.txt gzip_1 hexdump_1 hexdump_init
bandit12@bandit:/tmp/kruse3110$ xxd bzip_1
00000000: 1f8b 0808 0650 b45e 0203 6461 7461 322e  .....qQ.c..data2.
```
We identify the file signature yet again as gzip (1f8b)

### Extract gzip
```bash
bandit12@bandit:/tmp/kruse3110$ cp bzip_1 gzip_2.gz
bandit12@bandit:/tmp/kruse3110$ gzip -d gzip_2.gz
bandit12@bandit:/tmp/kruse3110$ ls
bzip_1 data.txt gzip_1 gzip_2 hexdump_1 hexdump_init
bandit12@bandit:/tmp/kruse3110$ head gzip_2
data5.bin0000644000000000000000000002400013655050006011240 0ustar  rootrootdata6.bin0000644000000000000000000000033613655050006011247 0ustar  rootrootBZh91AY&SY
                                          +
�A��z�<jA��j                               ���Y�@�U��Z��t!ހ��u�  �
```
We see that this file is a .ustar file, which is a type of archive that can be extracted using tar

### Extract .ustar
```bash
bandit12@bandit:/tmp/kruse3110$ cp gzip_2 compressed3.tar
bandit12@bandit:/tmp/kruse3110$ tar -xf compressed3.tar
bandit12@bandit:/tmp/kruse3110$ ls
bzip_1 compressed3.tar data5.bin data.txt gzip_1 gzip_2 hexdump_1 hexdump_init
bandit12@bandit:/tmp/kruse3110$ head data5.bin
���Y�@�U�0000644000000000000000000002400013655050006011240 0ustar  rootrootdata6.bin0000644000000000000000000000033613655050006011247 0ustar  rootrootBZh91AY&SY
```
By extracting the archive, we receive a new archive .ustar, so we extract as before

### Extract second .ustar file
```bash
bandit12@bandit:/tmp/kruse3110$ tar -xf data5.bin
bandit12@bandit:/tmp/kruse3110$ ls
bzip_1 compressed3.tar data5.bin data6.bin data.txt gzip_1 gzip_2 hexdump_1 hexdump_init
bandit12@bandit:/tmp/kruse3110$ head data6.bin
&SY�O����ڞOv���}?��}��^����������ߣ��;����▒4��▒h�F�F��4▒LM
                                                                               @��z��FM��C�hF�C@�4@f��h
4hh�▒=C%�>X▒,
bandit12@bandit:/tmp/kruse3110$ xxd data6.bin
00000000: 425a 6839 3141 5926 5359 8e4f 1cc8 0000  BZh91AY&SY.O....
```
We received data6.bin from the extraction and see that its again a bzip2 file (425a)

### Extract bzip2
```bash
bandit12@bandit:/tmp/kruse3110$ bzip2 -d data6.bin
bzip2: Can´t guess original name for data6.bin -- using data6.bin.out
```
```bash
bandit12@bandit:/tmp/kruse3110$ ls
bzip_1 compressed3.tar data5.bin data6.bin.out data.txt gzip_1 gzip_2 hexdump_1 hexdump_init
bandit12@bandit:/tmp/kruse3110$ head data6.bin.out
data8.bin000064400000000000000000011714304050561011244 0ustar rootrootqQcdata9.bin
```
The contents of data6.bin.out again seem to be an archive that we can extract using tar

### Extract .ustar using tar

```bash
bandit12@bandit:/tmp/kruse3110$ tar -xf data6.bin.out
bandit12@bandit:/tmp/kruse3110$ ls
bzip_1 compressed3.tar data5.bin data6.bin data8.bin data.txt gzip_1 gzip_2 hexdump_1 hexdump_init
bandit12@bandit:/tmp/kruse3110$ xxd data8.bin | head
00000000: 1f8b 0808 0650 b45e 0203 6461 7461 322e  .....qQ.c..data9.
```
Again a gzip file

### Extract using gzip
```bash 
bandit12@bandit:/tmp/kruse3110$ cp data8.bin data8.gz
bandit12@bandit:/tmp/kruse3110$ gzip -d data8.gz
bandit12@bandit:/tmp/kruse3110$ head data8
The password is wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw
```
After the gzip extraction, we try to read the file and finally we receive the contents.




## Level 13

<hr>

- Username: bandit13 <br>
- Password: wbWdlBxEir4CaE8LaPhauuOo6pwRmrDw <br>
- Task: The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on


```bash
bandit13@bandit:~$ ls
sshkey.private 
bandit13@bandit:~$ cat sshkey.private 
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
```
Either copy paste the key into a file on your local machine or use scp to download the key:
```bash
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
```

## Level 14

<hr>

- Username: bandit14 <br>
- Password: ssh key file  <br>
- Task: The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

```bash
krussve@kali:~$ chmod 700 sskey.private
krussve@kali:~$ ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14 
fGrHPx402xGC7U7rXKDaxiWFTOiFOENq 
```
Now we have the password for the current level, but need to provide this to port 30000 on localhost. We use nc which is a networking tool that, among others, allows us to send the password to open a connection to the port and then enter the password:
```bash
bandit14@bandit:~$ nc localhost 30000 
fGrHPx402xGC7U7rXKDaxiWFTOiFOENq 
Correct ! 
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt 
```

## Level 15

<hr>

- Username: bandit15 <br>
- Password: jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt <br>
- Task: The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.
  * Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…


```bash
bandit15@bandit:~$ openssl s_client -ign_eof -connect localhost:30001 
CONNECTED(00000003) 
Can´t use SSL get_servername 
depth=0 CN = localhost 
verify error:num=18:self-signed certificate 
verify return:1
...
jN2kgmIXJ6fShzhT2avhotn4Zcka6tnt 
Correct ! 
JQttfApK4SeyHwD119SXGR50qc10Ai11 
```


## Level 16

<hr>

- Username: bandit16 <br>
- Password: JQttfApK4SeyHwD119SXGR50qc10Ai11 <br>
- Task: The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.
  * From previous task: "Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…"
  
Make a port scan for ports 31000-32000 on the localhost:

```bash
bandit16@bandit:~$ nmap -A -p31000-32000 localhost 
Starting Nmap 7.80 ( https://nmap.org ) at 2022-11-02 18:27 UTC 
Nmap scan report for localhost (127.0.0.1) 
Host is up (0.00011s latency). 
Not shown: 996 closed ports 
PORT       STATE SERVICE VERSION 
31046/tcp  open  echo 
31518/tcp  open  ssl/echo 
PORT 
STATE     SERVICE VERSION 
31046/tcp open    echo 
31518/tcp open    ssl/echo 
| ssl-cert: Subject: commonName=localhost 
| Subject Alternative Name: DNS:localhost 
| Not valid before: 2022-11-01T21:06:02 
|_Not valid after: 2022-11-01T21:07:02 
31691/tcp open    echo 
31790/tcp open    ssl/unknown 
| fingerprint-strings: 
|   FourOhFourRequest, GenericLines, GetRequest, HTTPOptions, Help, Kerberos, LDAPSearchReq, LPDString, RTSPRequest, SIPOptions, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
|_  Wrong! Please enter the correct current password 
| ssl-cert: Subject: commonName=localhost 
| Subject Alternative Name: DNS:localhost 
| Not valid before: 2022-11-02T11:49:21 
|_Not valid after: 2022-11-02T11 : 50:21 
31960/tcp open    echo 
...
```

Only possible port seems to be 31790 so we try to connect
- need to use -ign_eof
```bash
bandit16@bandit:~$ openssl s_client -ign_eof localhost:31790 
CONNECTED(00000003) 
...
read R BLOCK 
JQttfApK4SeyHwD119SXGR50qc10Ai11 
Correct ! 
————BEGIN RSA PRIVATE KEY—————
M11Eog1BAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ 
imZzeyæøgtZPGujUSxiJSW1/oTqexh+cAMTSMIOJf7+BrJObArnxd9Y7YT2bRPQ 
Ja6Lzb558YW3FZ1870RiO+rW4LCDCNd21UvLE/GL2GWyuKNOK5iCd5TbtJzEkQTu 
DSt2mcNn4rhAL+JFr5604T6z8WWAW18BR6yGrMq7Q/kALHYW30ekePQAzLOVUYbW 
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjgOLWN6sK7wNX 
xOYVztz/zb1kPjfkU1jHS+9EbVNj+DIXFOJuaQIDAQABA01BABagpxpM1aoLWfvD 
KHcj10nqcoBc40E11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RILwDINhPx3iB1 
J9nOM80JOVToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P290vd 
d8WEryøgPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9A1bssgTcCXkMQnPw9nC 
YNN6DDP21bcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asv1pmS8A 
vLY9r60wYSvmZhNqBUrj71yCtXM1u1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama 
+TovwgEcgYEA8JtPxPOGRJ+1QkX262jM3dE1kza8ky5m01wUqYdsxONxH$RhORT 
8c8hAuRBb2G82s08vUHk/fur850Efc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx 
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd 
HCctNi/Fwju1httFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/bOiE7KaszX+Exdvt 
SghaTdcGOKnyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X315SiWg0A 
R57hJg1ez1iVjv3aGwHwv1ZvtszK6zV60XFAuOEcgYAbj046T4hyP5tJi93V5HDi 
Ttiek7xRVxU1+iU7rWkGAXFpMLFteQEsRr7PJ/1emmEY5eTDAFMLy9FL2m90QWCg 
R8VdwSk8r9FGLS+9aKcV5P1/WEK1wgXinB30hYimtiG2cg5JCq1ZFHxD6MjEGOiu 
L8ktHMPvodBwNsSBULpGOQKBgBAp1TfCIHOnWiMGOU3KPw',mt006CdTkmJOmL8Ni 
blh9e1yZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU 
YOdjHdSOoKvDQNWu6ucyLRAWFu1SeXw9a/9p7ftpxmOTsgyvmfLF2MIAEwyzRqaM 
77pBAoGAMmjmIJdjp+Ez8duyn3ie036yrttF5NSsJLAbxFpdlc1gvtGCH+9Cq0b 
dxviW8+TFVEB1104f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3 
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52061geuZ/ujbjY= 
————END RSA PRIVATE KEY————— 
```

## Level 17

<hr>

- Username: bandit17 <br>
- Password: ssh key <br>
- Task: There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new
  * NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

```bash
krussve@kali:~$ chmod 700 sskey.private
krussve@kali:~$ ssh -i sshkey.private bandit17@bandit.labs.overthewire.org -p 2220
```

compare the two files, then check if the data is in the new file.
```bash
bandit17@bandit:~$ ls 
passwords.new passwords.old 
bandit17@bandit:~$ diff passwords.new passwords.old 
42c42 
< hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg 
> 09wU1yMU4YhOz11LzxozOv01BzZ2TUAf 
bandit17@bandit:~$ cat passwords.new | grep hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg 
hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg
```

## Level 18

<hr>

- Username: bandit18 <br>
- Password: hga5tuuCLF6fFzUpnagiMN8ssu9LFrdg <br>
- Task: The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

Trying to login, but get:
```bash
krussve@kali:~$ ssh bandit18@bandit.labs.overthewire.org -p 2220
...
For support, questions or comments, contact us on discord or IRC. 
Enjoy your stay! 
Byebye ! 
Connection to bandit.labs.overthewire.org closed. 
```

By adding the command right after the ssh command, we might be able to execute it before the connection closes:
```bash
krussve@kali:~$ ssh bandit18@bandit.labs.overthewire.org -p 2220 ls
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org´s password: 
readme
```
```bash 
krussve@kali:~$ ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org´s password: 
awhqfNnAbc1naukrpqDYcF95h7HoMTrC
```



## Level 19

<hr>

- Username: bandit19 <br>
- Password: awhqfNnAbc1naukrpqDYcF95h7HoMTrC <br>
- Task: To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.


When executing the binary "./bandit20-do" we get the hint, that we are able to execute a command as another user. When we try the command "id" without the binary, we are only bandit19, but with the binary we receive the euid (effective user id which the process will use) bandit20! So now just extract the password using the binary:
```bash
bandit19@bandit:~$ ls
bandit20-do
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ id 
uid=11019(bandit19) gid=11019(bandit19) groups=11019(bandit19) 
bandit19@bandit:~$ ./bandit20-do id 
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19) 
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
VxCazJaVykI6W36BkBU0mJTCM8rR95XT
```

## Level 20

<hr>

- Username: bandit20 <br>
- Password: VxCazJaVykI6W36BkBU0mJTCM8rR95XT <br>
- Task: There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).
  * NOTE: Try connecting to your own network daemon to see if it works as you think

Using netcat, we listen to a connection (-l) on the port (-p) 1234 and send the password (echo ...) to the connection once established. The command is backgrounded by the "&".
```bash
bandit20@bandit:~$ echo 'VxcazJaVyk16W36BkBUømJTCN8rR95XT' | nc -l -p 1234 &
[1] 2619805 
bandit20@bandit:~$ ./suconnect 1234 
Read: VxcazJaVyk16W36BkBUømJTCM8rR95XT 
Password matches, sending next password 
NvEJF70Vjkdd1tpsrdKEF011h9V11Bcq 
[1] * Done 

```

## All commands and their description
<hr>

Use [man](https://man7.org/linux/man-pages/index.html) pages to find explanations and usage information.

| Command | Description |
|---------|-------------|
|    tr     |  translate or delete characters   |
|    xxd     |  xxd creates a hex dump of a given file or standard input   |
|    tar/bzip2/gzip     |  archiving/compression program   |
|     head    |  first 10 lines of output/file   |
|     ssh -i sshkeyfile   |  ssh connection with a ssh private key file   |
|    nmap     |  port and network scanner   |
|     diff    |  output the difference between two files   |