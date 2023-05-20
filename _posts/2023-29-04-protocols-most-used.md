---
title: "Protocols - most used"
date: 2023-04-29 18:25:00 +0100
categories: [Tools, Protocols]
tags: [linux, windows, tool guide, protocols]     # TAG names should always be lowercase
---

This is a work in progress containing information and usage of various protocols

----------

## Standard ports

| Protocol | TCP Port | Application(s)                    | Security  |
|----------|----------|-----------------------------------|-----------|
| FTP      | 21       | File Transfer                     | Cleartext |
| FTPS     | 990      | File Transfer                     | Encrypted |
| HTTP     | 80       | Worldwide Web                     | Cleartext |
| HTTPS    | 443      | Worldwide Web                     | Encrypted |
| IMAP     | 143      | Email (MDA)                       | Cleartext |
| IMAPS    | 993      | Email (MDA)                       | Encrypted |
| POP3     | 110      | Email (MDA)                       | Cleartext |
| POP3S    | 995      | Email (MDA)                       | Encrypted |
| SFTP     | 22       | File Transfer                     | Encrypted |
| SSH      | 22       | Remote Access and   File Transfer | Encrypted |
| SMTP     | 25       | Email (MTA)                       | Cleartext |
| SMTPS    | 465      | Email (MTA)                       | Encrypted |
| Telnet   | 23       | Remote Access                     | Cleartext |
|          |          |                                   |           |

----------

## SSH


##### General

- Default Port: 22
- SSH or "Secure SHell" characteristics:
  - Authentication: Confirm identity of remote server
  - Confidentiality: Encrypted (Predecessor telnet was not)
  - Integrity: Both parties can detect modifications to the message
- SCP (Secure Copy Protocol) can be used to transfer files and is based on SSH
  - Encrypted (Other than predecessor FTP)

##### Basic Usage

```bash
# Basic SSH connection
ssh <username>@<IP or Domain>

# With private key file
ssh -i path/to/private/key <username>@<IP> # Keys must be chmod 600 or 700 to use

# other options
ssh <username>@<IP> -p <port> # specify port if it differs from 22 
```

##### Advanced usage

```bash
# Reverse SSH port forwarding
# Port on remote server forwards to port on local server
ssh -L 9000:site.com:80 user@ip
## -L: You <- Client
## If a site like site.com is blocked, you could forward it's content to your own server and view it on localhost:9000

# SSH port forwarding
ssh -R 9000:site.com:80 user@ip
## -R: You -> Client
```

---

## HTTP


##### Response Codes

**2xx**: Success
- 200 **OK**: Request successful.

**3xx**: Redirect
- 301 **Moved Permanently**: Resource is moved to a new URL/path (permanently).
- 302 **Moved Temporarily**: Resource is moved to a new URL/path (temporarily).

**4xx**: Client error
- 400 **Bad Request**: Server didn't understand the request.
- 401 **Unauthorised**: URL needs authorisation (login, etc.).
- 403 **Forbidden**: No access to the requested URL. 
- 404 **Not Found**: Server can't find the requested URL.
- 405 **Method Not Allowed**: Used method is not suitable or blocked.
- 408 **Request Timeout**:  Request look longer than server wait time.

**5xx**: Server error
- 500 **Internal Server Error**: Request not completed, unexpected error.
- 503 **Service Unavailable**: Request not completed server or service is down.

----------

## FTP


File Transfer Protocol (FTP) is designed to transfer files with ease, so it focuses on simplicity rather than security. As a result of this, using this protocol in unsecured environments could create security issues.

##### Response Codes

FTP codes:
- "200" means successful
- x1x series: Information request responses.
  - 211: System status.
  - 212: Directory status.
  - 213: File status
- x2x series: Connection messages.
  - 220: Service ready.
	- 227: Entering passive mode.
	- 228: Long passive mode.
229: Extended passive mode
- x3x series: Authentication messages.
  - 230: User login.
	- 231: User logout.
	- 331: Valid username.
	- 430: Invalid username or password
  - 530: No login, invalid password.

----------

## DHCP


DHCP protocol, or Dynamic Host Configuration Protocol (DHCP), is used to manage automatic IP address and required communication parameters assignment.
- DHCP extends BOOTP, which is only capable of static IP address configuration



##### DHCP options

- DHCP options:
  - 3: DHCP Request: contains the hostname information
  - 5: DHCP ACK: packets represent the accepted requests 
  - 6: DHCP NAK: packets represent denied requests 

- DHCP Request options:
  - Option 12: Hostname.
	- Option 50: Requested IP address.
	- Option 51: Requested IP lease time.
  - Option 61: Client's MAC address.

- DHCP ACK options
  - 15: Domain name
  - 51: Assigned IP lease time

- DHCP NAK options
  - 56: Message (rejection details)