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
Command: cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m' 

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
bandit12@bandit:/tmp/kruse3110$
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
bzip2: Can't guess original name for data6.bin -- using data6.bin.out
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
- Task: 

```bash

```

## Level 11

<hr>

- Username: bandit <br>
- Password: bandit <br>
- Task: 

```bash

```

## Level 11

<hr>

- Username: bandit <br>
- Password: bandit <br>
- Task: 

```bash

```

## Level 11

<hr>

- Username: bandit <br>
- Password: bandit <br>
- Task: 

```bash

```

## Level 11

<hr>

- Username: bandit <br>
- Password: bandit <br>
- Task: 

```bash

```

## Level 11

<hr>

- Username: bandit <br>
- Password: bandit <br>
- Task: 

```bash

```

## Level 11

<hr>

- Username: bandit <br>
- Password: bandit <br>
- Task: 

```bash

```

## All commands and their description
<hr>

Use [man](https://man7.org/linux/man-pages/index.html) pages to find explanations and usage information.

| Command | Description |
|---------|-------------|
|    tr     |  translate or delete characters   |
|         |     |
|         |     |
|         |     |
|         |     |
|         |     |
|         |     |
|         |     |
|         |     |