---
layout: post
title: 🚗 Reverse Engineering Car CAN Bus – My First Automotive Hacking
date: 2025-06-24
categories: [Car hacking]
tag: [cybersecurity, hacking, writeups, CAN, reverseengineering]
---

![INTRODUCATION](/images/Reverse%20Engineering%20Car%20CAN%20Bus/intro.webp)

In this blog post, I’ll share my first hands on journey into automotive hacking by reverse engineering a car’s CAN (Controller Area Network) bus using virtual environments. I’ve always been curious about the hidden communication layers inside modern cars. I’ve done web hacking, malware analysis, OSINT, threat intelligence and you name it but automotive hacking felt like a completely new world. So I decided to dive in, and this is the story of my first real exploration into hacking the CAN bus.

It all started with setting up a virtual CAN (vcan) environment. I knew the basics of CAN, tiny data packets flying around between the different control units of a car like windows, doors, lights and all sending and receiving CAN IDs with specific data. Think of it as tiny notes being passed around between ECUs, each reacting when the note is addressed to them.

Starting small, I run [ICSim (Instrument Cluster Simulator) to simulate a car’s dashboard](https://github.com/zombieCraig/ICSim) and CAN network. I wanted to see if I could manually send CAN frames to control lights or doors, and then eventually build my automation scripts using python and bash progrogramming languages.


After experimenting, testing, and a bit of brute forcing, I managed to discover CAN IDs for some key controls:

✅Turning left and right indicators on and off

✅Unlocking the left door

✅Unlocking all doors at once

✅Locking the doors

✅Controlling speed of a car

## Sending the CAN Frame

Explanation of How I'm hacking the car in the virtual environment:

![infrastructure](/images/Reverse%20Engineering%20Car%20CAN%20Bus/infrastructure.webp)

I started by setting up a virtual CAN bus using vcan0 on Linux. This gave me a sandbox where I could simulate how real cars send and receive CAN messages. I installed can-utils, which gave me tools like cansend to send CAN frames and candump to watch traffic. Before brute forcing anything, I needed to see what kind of traffic normally flows through the simulated car dashboard (ICSim). So, I just ran candump vcan0 to get a feel for what IDs were active.

![candump](/images/Reverse%20Engineering%20Car%20CAN%20Bus/sample.webp)

## Brute-Force CAN 

Once I was ready, I wrote a brute force attack script using python and bash to start firing CAN frames at different IDs, one by one. At first, I used simple patterns in the payload just to see if anything responded visually on the car’s dashboard. Things like lights blinking, doors unlocking, or indicators turning on were my signs that I hit something important.

![starting-brute-force-attack](/images/Reverse%20Engineering%20Car%20CAN%20Bus/brute-force-main.webp)

Whenever I saw a reaction, I noted the CAN ID down. Then I manually tested it with cansend, changing the data payload slightly to see if I could toggle it ON and OFF. Sometimes a single byte like 01 at the start of the data could turn something ON, while 00 turned it OFF. It was trial and error, but that’s the fun part.

![bruteforceattack1](/images/Reverse%20Engineering%20Car%20CAN%20Bus/bruteforce-attack1.webp)

![bruteforceattack2](/images/Reverse%20Engineering%20Car%20CAN%20Bus/brute-force-another.webp)

Once I found something reliable, like a specific ID unlocking the left door or toggling indicators, I cleaned up the brute force script to focus on smaller ranges, and just used Python to automate turning things ON and OFF repeatedly so I could confirm what worked.

Through this process, I figured out how to control left and right indicators, unlock individual doors, and eventually even unlock all the doors at once.


## Left indicator

![left-indicator](/images/Reverse%20Engineering%20Car%20CAN%20Bus/left-indicator.webp)

## Right indicator

![right-indicator](/images/Reverse%20Engineering%20Car%20CAN%20Bus/right-indicator.webp)

## Unlock Left door
![left-door](/images/Reverse%20Engineering%20Car%20CAN%20Bus/unlock-left-door.webp)

## Unlock all doors
![unlock-all-doors](/images/Reverse%20Engineering%20Car%20CAN%20Bus/unlock-alldoors.webp)

## Lock all doors
![lock-all-doors](/images/Reverse%20Engineering%20Car%20CAN%20Bus/lock-all-doors.webp)

## Further Reading & Resources I Recommend

Along the way, I’ve been reading and learning from various resources to deepen my understanding of hacking, security, and specifically automotive exploitation. Here are a few books and links I highly recommend for anyone who wants to dive deeper:

[📘 “The Car Hacker’s Handbook” by Craig Smith](https://www.amazon.com/Car-Hackers-Handbook-Penetration-Tester/dp/1593277032)

[https://hackers-arise.com/automobile-hacking-the-ics-simulator-part-1/](https://hackers-arise.com/automobile-hacking-the-ics-simulator-part-1/)

[https://hackers-arise.net/2023/12/09/automobile-hacking-part-2-the-can-utils-or-socketcan/](https://hackers-arise.com/automobile-hacking-part-2-the-can-utils-or-socketcan/)


This is just the start for me. More to come soon.🥂😁