# 100-Day Pentesting Challenge
## Day 7: Nmap — The Pentester's Recon Swiss Army Knife

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** ~1 Hour

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| Nmap Scan Types | Know when to perform different scan techniques |
| Service & Version Detection | Identify running services and software versions |
| OS Fingerprinting | Learn about the target operating system |
| NSE Scripts | Automate enumeration and security checks |
| Output & Reporting | Save scan results for later analysis |

> **Every penetration test begins with reconnaissance.**
> Nmap helps identify hosts, services, operating systems, and potential attack surfaces.

---

## THM Rooms to Complete (In Order)

| # | Room | Link | Focus |
|---|---|---|---|
| 1 | Nmap | https://tryhackme.com/room/furthernmap | Core scan types, flags, output |
| 2 | Nmap Live Host Discovery | https://tryhackme.com/room/nmap01 | ARP scan, ping sweep, host discovery |
| 3 | Nmap Basic Port Scans | https://tryhackme.com/room/nmap02 | TCP, UDP, SYN scans |

> All rooms are available on TryHackMe.

---

## 1-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:10 | Read the theory below |
| 0:10 – 0:35 | Complete THM Rooms 1 & 2 |
| 0:35 – 0:58 | Complete THM Room 3 |
| 0:58 – 1:00 | Review the cheatsheet and update GitHub |

---

## Must-Know Concepts

### 1. Nmap Scan Types

| Scan | Flag | Description | Typical Use |
|---|---|---|---|
| SYN Scan | `-sS` | Half-open TCP scan | Fast reconnaissance |
| TCP Connect | `-sT` | Full TCP connection | When raw sockets aren't available |
| UDP Scan | `-sU` | Scan UDP services | DNS, SNMP, DHCP |
| Ping Sweep | `-sn` | Host discovery only | Find live systems |
| No Ping | `-Pn` | Skip host discovery | Scan hosts that don't reply to ICMP |
| NULL Scan | `-sN` | TCP packet without flags | Firewall testing |
| FIN Scan | `-sF` | FIN flag only | Firewall behavior analysis |
| Xmas Scan | `-sX` | FIN, PSH, URG flags | Advanced TCP scanning |
| ACK Scan | `-sA` | ACK packets | Firewall rule analysis |
| Version Detection | `-sV` | Identify service versions | Software identification |
| OS Detection | `-O` | Operating system fingerprinting | System identification |
| Aggressive Scan | `-A` | Enables multiple detection features | Comprehensive enumeration |

---

### 2. Target Specification

```bash
nmap 192.168.1.1
nmap 192.168.1.1-20
nmap 192.168.1.0/24
nmap 10.0.0.0/8
nmap -iL targets.txt
nmap --exclude 192.168.1.1
nmap scanme.nmap.org
```

---

### 3. Port Specification

```bash
nmap -p 80 target
nmap -p 22,80,443 target
nmap -p 1-1000 target
nmap -p- target
nmap -p- --min-rate 5000 target
nmap -F target
nmap --top-ports 500 target
```

---

### 4. Timing Templates

```bash
nmap -T0 target
nmap -T1 target
nmap -T2 target
nmap -T3 target
nmap -T4 target
nmap -T5 target
```

| Template | Description |
|---|---|
| `T0` | Paranoid |
| `T1` | Sneaky |
| `T2` | Polite |
| `T3` | Normal (default) |
| `T4` | Aggressive |
| `T5` | Insane |

---

### 5. Service & OS Detection

```bash
nmap -sV target

nmap -sV --version-intensity 9 target

nmap -O target

nmap -A target
```

**Example Output**

```text
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2
80/tcp open  http    Apache httpd 2.4.18
```

---

### 6. Nmap Scripting Engine (NSE)

Nmap includes hundreds of built-in scripts for enumeration, information gathering, and security assessment.

#### Script Categories

| Category | Purpose |
|---|---|
| `auth` | Authentication checks |
| `default` | Safe default scripts |
| `discovery` | Information gathering |
| `exploit` | Exploit-related checks |
| `vuln` | Vulnerability detection |
| `brute` | Credential testing |
| `safe` | Non-intrusive scripts |

#### Common NSE Commands

```bash
nmap -sC target

nmap --script vuln target

nmap --script smb-vuln-ms17-010 -p 445 target

nmap --script ftp-anon -p 21 target

nmap --script http-enum -p 80 target

nmap --script http-title -p 80 target

nmap --script ssh-brute -p 22 target

nmap --script dns-zone-transfer --script-args dns-zone-transfer.domain=target.com target

nmap --script smb-enum-shares -p 445 target

nmap --script mysql-info -p 3306 target

nmap --script http-shellshock -p 80 target
```

---

### 7. Output Formats

```bash
nmap -oN scan.txt target

nmap -oX scan.xml target

nmap -oG scan.gnmap target

nmap -oA scan target

grep "open" scan.gnmap
```

---

### 8. Example Recon Workflow

```bash
# Discover live hosts
nmap -sn 192.168.1.0/24

# Scan common ports
nmap --top-ports 1000 -T4 192.168.1.50

# Scan every TCP port
nmap -p- -T4 192.168.1.50

# Detect versions and OS
nmap -sV -sC -O -p 22,80,445 192.168.1.50

# Run vulnerability scripts
nmap --script vuln -p 22,80,445 192.168.1.50

# Save all results
nmap -A -oA full_recon 192.168.1.50
```

---

### 9. Port States

| State | Meaning |
|---|---|
| `open` | Service is accepting connections |
| `closed` | Host is reachable but no service is listening |
| `filtered` | Firewall or filtering device prevented determination |
| `open\|filtered` | Unable to distinguish between open and filtered |
| `unfiltered` | Reachable, but state cannot be determined |

---

### 10. Useful Nmap Commands

```bash
# Comprehensive lab scan
nmap -sV -sC -A -T4 -p- target

# SYN scan
nmap -sS target

# Scan specific ports
nmap -Pn -p 22,80,443,8080 target

# Scan web ports on a subnet
nmap -p 80,443,8080,8443 192.168.1.0/24 --open

# Vulnerability overview
nmap -sV --script vuln target
```

---

## Quick Reference Cheatsheet

```text
Discovery:
nmap -sn 192.168.1.0/24

SYN Scan:
nmap -sS target

Full Scan:
nmap -p- -T4 target

Version Detection:
nmap -sV target

OS Detection:
nmap -O target

Aggressive Scan:
nmap -A target

Default Scripts:
nmap -sC target

Vulnerability Scripts:
nmap --script vuln target

Save Results:
nmap -oA output target

Timing:
-T4 (Labs)
-T2 or T3 (Slower Scans)

Port States:
open
closed
filtered
open|filtered
unfiltered
```

---

## Day 7 Submission Task

```text
Complete all TryHackMe rooms listed above

Run an Nmap scan against a lab machine

Capture a screenshot showing discovered ports and services

Push your work to:
100-days-pentest/Day-07/

Share your progress on Discord or Telegram
```

---

> **End of Day 7 — Phase 1 Networking Week Complete**

**Next Up:** Day 8 — OWASP Top 10: Introduction to Common Web Application Security Risks

---
