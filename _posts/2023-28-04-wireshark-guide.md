---
title: "Wireshark Guide"
date: 2023-04-28 18:25:00 +0100
categories: [Tools, Network]
tags: [wireshark, blue team, network, security, tool guide]     # TAG names should always be lowercase
---

A guide to operators, filters and other usage of wireshark.

# Introduction

Wireshark is used to capture and analyze network traffic through <abbr title="Packet Capture">PCAPs</abbr>.
This guide will contain chapters for working methodology, general filtering options and protocol specific filtering options.

# Methodology

Depending on the PCAP and what you are looking for, different methodologies are necessary.

TO get a quick overview of the data at hand, use the **Statistics** tab, which shows aggregated and statistical information such as protocols used, IPs, Conversations, and more.
Using these satistics, you can find the following information:
- Used IP addresses and whether any are suspicious
- Are suspicious ports involved?
- Any abnormal usage and behavior

### DNS analysis

- Go through the DNS queries and replies 

Look for the following:
- Used domains
- Suspicious destinations involved?
- Anything other unusual about the queries


### HTTP analysis 

- Go through GET requests

Look for the following:
- What addresses are used
- Resource share event (File-sharing etc.)
- Unusual user-agents (e.g. Kali Linux)
- Check requested URIs

### Files in HTTP

- "File --> Export Object --> HTTP"
  - Extract files
  - Use Hashes for analysis
  - What domain hosted the file

# Operators

## Basic Operators

| Operator | Description |
| :--- | :--- | 
| **and** (**&&**) | and operators |
| **or** (**\|\|**) | or operators |
| **eq** (**==**) | equals operators |
| **!=** / **>** / **<** | not equals / greater than / less than |

## Advanced Operators

| Filter | Description | Example |
| :--- | :--- | :--- |:--- |
| contains | Case-sensitive search inside packets | http.server contains "Nginx" |
| matches  | Search inside with Regex. Case-insensitive | http.host matches "\\.(php\|html) |
| in | Search using a list/range | tcp.port in {80 443 8080} |
| upper | Convert string to uppercase (makes search easier) | upper(http.server) contains "NGINX" |
| lower | Convert string to lowercase | lower(http.server) contains "nginx" |
| string | Convert field to string | string(frame.number) matches "[1843]$" |


# Filtering options

## IP filters

| Filter          | Description                                       |
|:----|:---|
| ip              | Show all IP packets                               |
| ip.addr == <ip> | Show all packets for given address                |
| ip.src / dst    | Show all packets from / to address                |
| addr vs src/dst | addr filters without considering packet direction |

- Can use explicit IP addresses, subnets or lists, see [advanced operators](#advanced-operators)

## TCP and UDP filters

| Filter | Description |
| :--- | :--- | 
| tcp.port | All packets using the port |
| tcp.srcport | All packets originating from this port |
| tcp.dstport | All packets destined to this port |

- Switch "tcp" with "udp" for the same result

## HTTP

| Filter | Description |
| :--- | :--- | 
| http ot http2 | Show all HTTP packets |
| http.response.code == 404 | Show packets with given code (also posisble to use >=200) |
| http.request.method == "GET" | ALl "GET" packets |
| http.user_agent  |  User agent |
| http.request.uri contains "keyword"  | Will show "/subpage"  |
| http.request.full_uri contains "admin"  | Will show "full.uri.org"  |
| http.host   | host such as website  |
|  http.server | Server  |
| data-text-lines contains "keyword" | Describes content (e.g. text/html or image/jpg)  |

- See [Protocols - most used](../posts/2023-29-04-protocols-most-used.md) for HTTP codes

## DNS

| Filter | Description |
| :--- | :--- | 
| dns | Show all DNS packets |
| dns.flags.response == 0 | Show all DNS requests |
| dns.flags.response == 1 | Show all DNS reponses |
| dns.qry.type == 1 | Show all DNS "A" records |



## ARP

| Filter | Description  |
|---|---|
| arp  | Global search   |
| arp.opcode == 1  | ARP requests  |
| arp.opcode == 2  | ARP responses  |
| arp.duplicate-address-detected or arp.duplicate-address-frame  | Possible ARP poisoning  |
| ((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == target-mac-address)  |  Possible ARP flooding  |
| arp.dst.hw_mac==00:00:00:00:00:00  | ARP scanning  |

## FTP


| Filter   | Description  |
|---|---|
| ftp.request.code == xxx  | Get packets with certain request code  |
| ftp.request.command == "USER/PASS" | Find packets by used commands  |
| ftp.request.arg == "password"  | Find packets with certain arguments  |

- See [Protocols - most used](../posts/2023-29-04-protocols-most-used.md) for FTP codes

## HTTPS

| Filter | Description |
|--------|-------------|
|   (http.request or tls.handshake.type == 1) and !(ssdp)   |  Client Hello  |
| (http.request or tls.handshake.type == 2) and !(ssdp)  | Server Hello   |


## DHCP

| Filter | Description |
|--------|-------------|
|  dhcp or bootp      |    Global Search         |
|  dhcp.option.dhcp == xx | Filter by DHCP option  |
|   dhcp.option.hostname contains "keyword"  |    Filter by hosname         |
|   dhcp.option.domain_name contains "keyword" |  Filter by domain name           |


- See [Protocols - most used](../posts/2023-29-04-protocols-most-used.md) for DHCP options



## NetBIOS

| Filter | Description |
|--------|-------------|
|     nbns   |  Global search           |
|   nbns.name contains "keyword"     |  Filter by nbns name  |

## Kerberos

| Filter | Description |
|--------|-------------|
|   kerberos     | Global search             |
| kerberos.CNameString contains "keyword"  | Filter by username/hostname  |
|  kerberos.pvno == 5      |    Filter by protocol version         |
|  kerberos.realm contains ".org"     |   Domain name for generated ticket |
| kerberos.SNameString == "krbtg" | Service and domain name for ticket |
| kerberos.addresses | Client IP and NetBIOS name (Only in request packet) |


- Filtering by username: 
  - **kerberos.CNameString and !(kerberos.CNameString contains "$")**
  - CNameString with "$" is hostname, without is username

## NMAP

| Filter  | Description  |
|---|---|
| icmp.type==3 and icmp.code==3      | UDP Scan  |
| tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size <= 1024  | SYN Scan  |
| tcp.flags.syn==1 and tcp.flags.ack==0 and tcp.window_size > 1024   | TCP Connect scan  |


## User Agent

|  Filter | Description   |
|---|---|
| http.user_agent  | all user agent fields  |
| http.user_agent contains "sqlmap/Nmap/Wfuzz/Nikto"  | Search for tools  |

- Search for anomalies
  - Misspelling
  - Tools
  - Different user-agents from the same source in a short time
  - Payload data