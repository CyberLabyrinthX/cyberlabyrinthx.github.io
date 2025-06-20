---
layout: post
title: Anonymous TryHackme Challenge - Writeup
date: 2025-06-20
categories: [TRYHACKME-WRITEUPS]
tag: [TRYHACKME, HACKING, CYBERSECURITY, WRITEUP, ANONYMOUS]
---

![introduction](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a6/Anonymous_emblem.svg/1024px-Anonymous_emblem.svg.png)

Anonymous Tryhackme challenge - [click here to access the challenge](https://tryhackme.com/room/anonymous)

## TASK DESCRIPTION:


Try to get the two flags!  Root the machine and prove your understanding of the fundamentals!

[Answer the questions below](https://tryhackme.com/room/anonymous)

## NMAP SCAN 
Enumerate the machine.  How many ports are open?

```terminal
nmap -sS -sC ipv4_addr -F -T4
```

![screenshot1](/images/tryhackme-anonymous/nmap_results.webp)

## SMB Enumeration 

What service is running on port 21?

```Answer
File Transfer Protocol (FTP)
```

What service is running on ports 139 and 445?

```Answer
Server Message Block (SMB) protocol
```

There's a share on the user's computer.  What's it called?

```answer
pics
```

To explore SMB shares, I used a tool called Enum4linux to enumerate the available shares. It’s a powerful tool that helps gather information from SMB on both Windows and Linux systems. 

![screenshot2](/images/tryhackme-anonymous/enum4linux.webp)

Then enumerated usernames, groups, shared folders (SMB shares), hostnames, operating system info, and  domain/workgroup details.

![screenshot3](/images/tryhackme-anonymous/user_enum.webp)

## Data exfiltration using Smbclient

![screenshot4](/images/tryhackme-anonymous/smb_download.webp)

## Gaining access via FTP and data exfiltration

Inside the server, there was a folder called scripts, and it contained a file named clean.sh.

The file had the permission -rwxr-xrwx, meaning everyone had write and execute access. This allowed me to download the script, modify it, and upload it back with my own reverse shell code. 

![screenshot5](/images/tryhackme-anonymous/ftp-access.webp)

![screenshot6](/images/tryhackme-anonymous/ftp-data.webp)

![screenshot7](/images/tryhackme-anonymous/ftp-exfil.webp)

## Establishing a foothold

Since the script was likely executed by a higher privileged user or automatically by the system, it gave me a foothold on the machine when triggered.

![screenshot8](/images/tryhackme-anonymous/ftp-foothold.webp)

![sceenshot9](/images/tryhackme-anonymous/shell-access.webp)

## Shell stabilisation

After getting a basic reverse shell, it was very limited. I couldn’t use commands like clear, sudo, or navigate easily. So I stabilized the shell to make it more usable, like a normal terminal.

![screenshot10](/images/tryhackme-anonymous/stabilization.webp)

![user_flag](/images/tryhackme-anonymous/user_flag.webp)

## Privilege Escalation to root 

After gaining a foothold, I had access as a low privileged user. To escalate privileges, I searched the system for files with the SUID bit set, which can allow users to execute files with the permissions of the file’s owner often root. Using the command 
```console
find / -perm -4000 -type f 2>/dev/null
```
I found several SUID binaries, including some that are known to be vulnerable. I [chose to exploit the env SUID binary](https://gtfobins.github.io/gtfobins/env), which allowed me to execute a shell as root, and I achieved privilege escalation.

![screenshot11](/images/tryhackme-anonymous/priv-esc.webp)

![screenshot12](/images/tryhackme-anonymous/root.webp)

With root privileges, I navigated to the root directory and read the final flag

![screenshot13](/images/tryhackme-anonymous/root2.jpg)



