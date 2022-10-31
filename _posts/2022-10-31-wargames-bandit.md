---
title: "OverTheWire Wargames: Bandit Level 0-10"
date: 2022-10-31 18:25:00 +0100
categories: [Wargames, Bandit]
tags: [bandit, linux, overthewire, wargames, commandline]     # TAG names should always be lowercase
---


My Write-up of the infamous [Bandit](https://overthewire.org/wargames/bandit/) challenges, that "can help you to learn and practice security concepts in the form of fun-filled games."

# General
<hr>

Usernames have the following format: bandit\<level>, so bandit0, bandit1 ...

To connect the following command is used:
```bash
$ ssh bandit<level>@bandit.labs.overthewire.org -p 2220
```
Then you will be asked to give the passwords, which you will find within each level.

## Level 0

<hr>

- **Username: bandit0** (given) <br>
- **Password: bandit0** (given) <br>
- Task: The password for the next level is stored in a file called readme located in the home directory.

```bash
banditEbandit:~$ ls 
readme 
bandit0bandit:~$ cat readme 
NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL 
```

## Level 1

<hr>

- Username: bandit1 <br>
- Password: NH2SXQwcBdpmTEzi3bvBHMM9H66vVXjL <br>
- Task: The password for the next level is stored in a file called "-" located in the home directory.

```bash
bandit1@bandit:~$ ls 
-
bandit1@bandit:~$ cat ./-
rRGizSaX8Mk1RTb1CNQoXTcYZWU61gzi 
```

## Level 2

<hr>

- Username: bandit2 <br>
- Password: rRGizSaX8Mk1RTb1CNQoXTcYZWU61gzi <br>
- Task: The password for the next level is stored in a file called "spaces in this filename" located in the home directory

```bash
bandit2@bandit:~$ ls 
spaces in this filename 
bandit2@bandit:~$ cat "spaces in this filename" 
aBZOW5EmUfAf7kHTQeOwd8bauFJ21AiG 
```

## Level 3

<hr>

- Username: bandit3 <br>
- Password: aBZ0W5EmUfAf7kHTQeOwd8bauFJ2lAiG <br>
- Task: The password for the next level is stored in a hidden file in the inhere directory.

```bash
bandit3@bandit:~$ ls 
inhere 
bandit3@bandit:~$ cd inhere 
bandit3@bandit:~/inhere$ ls 
bandit3@bandit:~/inhere$ ls -la 
total 12 
drwxr-xr-x 2 root root 4096 sep 1 06:30 .
drwxr-xr-x 3 root root 4096 sep 1 06:30 ..
-rw-r 1 bandit4 bandit3  33 sep 1 06:30 .hidden
bandit3@bandit:~/inhere$ cat .hidden 
2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe 
```

## Level 4

<hr>

- Username: bandit4 <br>
- Password: 2EW7BBsr6aMMoJ2HjW067dm8EgX26xNe <br>
- Task: The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

```bash
bandit4@bandit:~$ ls
inhere 
bandit4@bandit:~$ cd inhere 
bandit4@bandit:~/inhere$ ls 
-file00 -file01 -file02 -file03 -file04 -file05 -file06 -file07 -file08 -file09 
bandit4@bandit:~/inhere$ cd ..
bandit4@bandit:~$ find ./inhere -type f -exec file { } + I grep ASCII 
./inhere/-file07: ASCII text 
bandit4@bandit:~$ cat ./inhere/-file07 
Ir1WW16bB37kxfiCQZqUd01Yfr6eEeqR 

```

## Level 5

<hr>

- Username: bandit5 <br>
- Password: Ir1WW16bB37kxfiCQZqUd01Yfr6eEeqR <br>
- Task: The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:
  * human-readable
  * 1033 bytes in size
  * not executable

```bash
bandit5@bandit:~$ ls
inhere 
bandit5@bandit:~$ ls inhere 
maybehere00 maybehere03 maybehere06 maybehere09 maybehere12 maybehere15 maybehere18 
maybehere01 maybehere04 maybehere07 maybehere10 maybehere13 maybehere16 maybehere19 
maybehere02 maybehere05 maybehere08 maybeherell maybehere14 maybehere17 
bandit5@bandit:~$ find ./inhere -type f ! -executable -size 1033c -exec file { } + I grep ASCII 
./inhere/maybehere07/.file2: ASCII text, with very long lines (1000) 
bandit5@bandit:~$ cat ./inhere/maybehere07/.file2 
P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU 
```

## Level 6

<hr>

- Username: bandit6 <br>
- Password: P4L4vucdmLnm8I7Vl7jG1ApGSfjYKqJU <br>
- Task: The password for the next level is stored somewhere on the server and has all of the following properties:
  - owned by user bandit7
  - owned by group bandit6
  - 33 bytes in size

```bash
bandit6@bandit:~$ ls 
bandit6@bandit:~$ ls -la 
total 20 
drwxr-xr-x  2 root root 4096 Sep 1 06:30 .
drwxr-xr-x 49 root root 4096 Sep 1 06:30 ..
-rw-r--r--  1 root root  220 Jan 6  2022 .bash_logout
-rw-r--r--  1 root root 3771 Jan 6  2022 .bashrc 
-rw-r--r--  1 root root  807 Jan 6  2022 .profile 
bandit6@bandit:~$ cd ..
bandit6@bandit:/home$ cd .. 
bandit6@bandit:/$ ls 
bin  dev home    lib   lib64  lost+found mnt proc run  snap sys usr
boot etc krypton lib32 libx32 media      opt root sbin srv  tmp var
bandit6@bandit:/$ find ./ -user bandit7 -group bandit6 -size 33c 2>/dev/null 
./var/lib/dpkg/info/bandit7.password 
bandit6@bandit:/$ cat ./var/lib/dpkg/info/bandit7.password 
z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S 
```

## Level 7

<hr>

- Username: bandit7 <br>
- Password: z7WtoNQU2XfjmMtWA8u5rN4vzqu4v99S <br>
- Task: The password for the next level is stored in the file data.txt next to the word millionth

```bash
bandit7@bandit:~$ ls  
data.txt 
bandit7@bandit:~$ cat data.txt | grep "millionth" 
millionth       TESKZC0XvTetK0S9xNwm25STk5iWrBvP 
```

## Level 8

<hr>

- Username: bandit8 <br>
- Password: TESKZC0XvTetK0S9xNwm25STk5iWrBvP <br>
- Task: The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

```bash
bandit8@bandit:~$ ls 
data.txt
bandit8@bandit:~$ cat data.txt | sort | uniq -u 
EN632PlfYiZbn3PhVK3XOGSlNInNE00t
```

## Level 9

<hr>

- Username: bandit9 <br>
- Password: EN632PlfYiZbn3PhVK3XOGSlNInNE00t <br>
- Task: The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

```bash
bandit9@bandit:~$ strings data.txt | grep "=="
========== the 
bu========== password 
4iu========== is 
b~==P
=========== G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s 
```

## Level 10

<hr>

- Username: bandit10 <br>
- Password: G7w8LIi6J3kTb8A7j9LgrywtEUlyyp6s <br>
- Task: The password for the next level is stored in the file data.txt, which contains base64 encoded data.

```bash
bandit10@bandit:~$ ls 
data.txt 
bandit10@bandit:~$ base64 -d data.txt 
The password is 6zPeziLdR2RKNdNYFNb6nVCKzphlXHBM 
```