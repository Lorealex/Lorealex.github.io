---
layout: single
title: OnSystemShellDredd - Proving grounds Play
date: 2023-06-13
classes: wide
header:
  teaser: /assets/images/linux.png
categories:
  - provinggrounds
  - infosec
tags:
  - provinggrounds
  - linux
  - ftp
  - ssh
  - cpulimit
  - suid
  - easy
---

## Linux

![](/assets/images/linux.png)

This blog post is a writeup of the OnSystemShellDredd machine from Proving grounds play.

### Summary
------------------
- FTP service allows us to access as anonymous user and obtain a RSA SSH key.
- This key can be used to access via SSH.
- Having access an attacker can exploit a command with the SUID to elevate privileges.

### Detailed steps
------------------

### Enumeration

This machine has only two open ports --> 21-FTP and 61000-SSH

I started as always just enumerating the open ports with:
```
nmap -Pn -p- 192.168.187.130
```
![nmap1](\assets\images\pg-play-onsystemshelldredd\0.JPG)

Knowing which ports are open, I executed an agressive nmap with:
```
nmap -A --script=vuln 192.168.247.130 -p 21,61000 -o nmapa.txt
```
![nmap1](\assets\images\pg-play-onsystemshelldredd\01.JPG)

### Explotation

In order to gain access I tried to login as anonymous user in FTP service.
I was able to access and find a SSH key:
![ftp](\assets\images\pg-play-onsystemshelldredd\2.JPG)

I downloaded the SSH 
![ftp2](\assets\images\pg-play-onsystemshelldredd\3.JPG)

This key allowed me to login via SSH as "hannah" with:
```
ssh -i id_rsa hannah@192.168.187.130 -p 61000
```
![ftp3](\assets\images\pg-play-onsystemshelldredd\6.JPG)

Having access as hannah, I was able to find the user flag:
![userf](\assets\images\pg-play-onsystemshelldredd\7.JPG)

### Privilege scalation

In order to elevate privileges I used Linpeas.sh --> https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS
Linpeas found an interesting command with `SUID` : cpulimit

As we can see in the following picture, I was able to execute a root bash shell with cpu limit and find the root flag:
![root](\assets\images\pg-play-onsystemshelldredd\21.JPG)

Root flag:
![root2](\assets\images\pg-play-onsystemshelldredd\22.JPG)