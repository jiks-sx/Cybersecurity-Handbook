# 100-Day Pentesting Challenge
## Day 4: Pre-Security — Network Fundamentals

> **Platform:** TryHackMe (THM) + Cisco SkillsForAll &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** ~1 Hour

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| TCP/IP Model | How the internet actually works under the hood |
| OSI Model | Universal framework — every interview asks this |

> Almost **every cyber attack happens over a network.**
> XSS, SQLi, reverse shells, phishing — all travel through protocols you'll learn today.
> Understand how data moves to learn how to intercept and manipulate it.

---

## Lab 1 — TryHackMe (Warm-Up)

| # | Room | Link | Focus |
|---|---|---|---|
| 1 | What is Networking? | https://tryhackme.com/room/whatisnetworking | Network basics, LAN, WAN |

> Part of the **Pre-Security Path** on THM — completely free.

---

## Lab 2–5 — Cisco SkillsForAll (Main Resource)

> **Why Cisco over THM for the rest of today?**
> Cisco built much of the networking technology used across the internet. Their free courses include interactive labs, Packet Tracer simulations, quizzes, and shareable completion badges.

| # | Course / Module | Where to Find | Focus |
|---|---|---|---|
| 2 | Networking Basics — Module 1 & 2 | Search **"Skills For All Cisco Networking Basics"** on https://skillsforall.com | Network types, devices, topologies |
| 3 | Networking Basics — Module 3 & 4 | Same course | OSI & TCP/IP models with interactive labs |
| 4 | Networking Basics — Module 5 & 6 | Same course | IP addressing, subnetting basics |
| 5 | Networking Devices & Initial Config | Search **"Skills For All Cisco Networking Devices"** on https://skillsforall.com | Routers, switches, NAT, VLANs |

- 100% Free — No credit card required
- Includes a completion badge for LinkedIn
- Packet Tracer labs included for hands-on practice

---

## 1-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:10 | Read the theory below (TCP/IP + OSI quick reference) |
| 0:10 – 0:20 | Complete THM Lab 1 — What is Networking? |
| 0:20 – 0:35 | Cisco SkillsForAll — Networking Basics Modules 1 & 2 |
| 0:35 – 0:50 | Cisco SkillsForAll — Networking Basics Modules 3 & 4 |
| 0:50 – 1:00 | Review the cheatsheet and update GitHub |

---

## Must-Know Concepts

### 1. OSI Model — 7 Layers

```text
┌──────────────────────────────────────────────────────────────┐
│ Layer 7 — Application   │ HTTP, HTTPS, FTP, DNS, SMTP       │
│ Layer 6 — Presentation  │ SSL/TLS, Encryption, Compression  │
│ Layer 5 — Session       │ Sessions, Authentication          │
│ Layer 4 — Transport     │ TCP, UDP, Port Numbers            │
│ Layer 3 — Network       │ IP Addresses, Routing, ICMP       │
│ Layer 2 — Data Link     │ MAC Addresses, ARP, Switches      │
│ Layer 1 — Physical      │ Cables, Hubs, Wi-Fi Signals       │
└──────────────────────────────────────────────────────────────┘
```

**Mnemonic (Bottom → Top):**

**P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way

### Focus Layers for Pentesting

| Layer | What to Know | Attack Example |
|---|---|---|
| Layer 7 — Application | HTTP, DNS, FTP | SQL Injection, XSS, SSRF |
| Layer 4 — Transport | TCP vs UDP, Ports | Port scanning, SYN flood |
| Layer 3 — Network | IP addresses, Routing | IP spoofing |
| Layer 2 — Data Link | MAC addresses, ARP | ARP spoofing, Man-in-the-Middle |

---

### 2. TCP/IP Model — 4 Layers

| TCP/IP Layer | OSI Equivalent | Key Protocols |
|---|---|---|
| Application | Layers 5–7 | HTTP, DNS, FTP, SSH, SMTP |
| Transport | Layer 4 | TCP, UDP |
| Internet | Layer 3 | IP, ICMP, ARP |
| Network Access | Layers 1–2 | Ethernet, Wi-Fi |

> **OSI = Theory (7 layers). TCP/IP = Practical implementation (4 layers). Learn both and understand how they map to each other.**

---

### 3. TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | 3-way handshake | Connectionless |
| Reliability | Guaranteed delivery | No guarantee |
| Speed | Slower | Faster |
| Common Uses | HTTP, SSH, FTP, SMB | DNS, VoIP, Streaming, Gaming |

**TCP Three-Way Handshake**

```text
Client ──── SYN ────────────► Server
Client ◄─── SYN-ACK ───────── Server
Client ──── ACK ────────────► Server
         [Connection Established]
```

> **Pentest Note:** `nmap -sS` performs a SYN scan by sending the initial SYN packet without completing the full TCP handshake.

---

### 4. Private IP Address Ranges

```text
Class A → 10.0.0.0      – 10.255.255.255
Class B → 172.16.0.0    – 172.31.255.255
Class C → 192.168.0.0   – 192.168.255.255
```

> If you compromise a machine with an address such as `10.x.x.x`, `172.16.x.x`, or `192.168.x.x`, it is part of a private network that may contain additional internal systems for enumeration.

---

## Quick Reference Cheatsheet

```text
OSI (7):     Application → Presentation → Session → Transport → Network → Data Link → Physical

TCP/IP (4):  Application → Transport → Internet → Network Access

TCP:         Reliable | 3-Way Handshake | ACK | HTTP, SSH, SMB

UDP:         Fast | Connectionless | No ACK | DNS, VoIP, Streaming

Private IPs: 10.x.x.x | 172.16–31.x.x | 192.168.x.x

/24 Subnet:  255.255.255.0 = 254 usable hosts

Common Ports:
22   SSH
53   DNS
80   HTTP
443  HTTPS
445  SMB
3389 RDP
```

---

## Day 4 Submission Task

```text
Complete THM Lab 1 (What is Networking?)

Complete Cisco SkillsForAll Networking Basics (Modules 1–4 minimum)

Take a screenshot of the completed THM room

Take a screenshot of your Cisco SkillsForAll progress or badge

Push both screenshots to:
100-days-pentest/Day-04/

Share your progress in the daily-progress channel on Discord or Telegram
```

> **Bonus:** If you earn the Cisco Networking Basics badge, share it on LinkedIn using **#HackerHub** and **#100DayPentest**.

---

> **End of Day 4**

**Next Up:** Day 5 — How the Web Works (HTTP, DNS, and URLs)

---
