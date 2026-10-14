# 100-Day Pentesting Challenge
## Day 8: Further Nmap — NSE Scripts, UDP & OS Detection

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| NSE Scripts | Automate enumeration, vulnerability detection, and exploitation checks |
| UDP Scanning | Many important services use UDP and are often overlooked |
| OS Detection | Identify the target operating system before selecting an attack path |
| Firewall Evasion | Understand techniques used to reduce scan visibility |
| Output & Reporting | Document findings professionally |

> **Day 7 introduced the fundamentals of Nmap.**
> Today you'll explore advanced scanning techniques, NSE scripts, UDP scanning, OS fingerprinting, and reporting.

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 1:00 | Study NSE scripts and script categories |
| 1:00 – 1:30 | Learn UDP scanning and OS detection |
| 1:30 – 2:00 | Study firewall evasion techniques |

---

## Must-Know Concepts

### 1. NSE — Nmap Scripting Engine Deep Dive

The **Nmap Scripting Engine (NSE)** transforms Nmap from a simple port scanner into a powerful reconnaissance framework.

Scripts are written in **Lua** and can perform:

- Service enumeration
- Version detection
- Vulnerability detection
- Authentication testing
- Information gathering
- Exploit checks

**Script Location (Linux):**

```bash
ls /usr/share/nmap/scripts/

ls /usr/share/nmap/scripts/ | wc -l
```

---

### 2. NSE Script Categories

| Category | Purpose | Risk |
|---|---|---|
| `safe` | Safe information gathering | Safe |
| `default` | Default scripts used with `-sC` | Safe |
| `discovery` | Service and host discovery | Safe |
| `auth` | Authentication checks | Moderate |
| `brute` | Credential brute force | Moderate |
| `vuln` | Vulnerability detection | Moderate |
| `exploit` | Exploit-related checks | High |
| `intrusive` | May modify or disrupt services | High |
| `malware` | Malware and backdoor detection | Moderate |
| `external` | Uses third-party services | Moderate |

---

### 3. Running NSE Scripts

```bash
# Default scripts
nmap -sC target

# Single script
nmap --script http-title target

# Multiple scripts
nmap --script "http-title,http-headers" target

# Script category
nmap --script vuln target
nmap --script brute target
nmap --script auth target

# Wildcards
nmap --script "http-*"
nmap --script "ftp-*"
nmap --script "smb-*"

# Multiple categories
nmap --script "default,safe" target

# Script arguments
nmap --script http-brute \
--script-args userdb=users.txt,passdb=pass.txt target

# Script help
nmap --script-help http-shellshock
```

---

### 4. Essential NSE Scripts

#### HTTP

```bash
nmap --script http-title -p 80,443,8080 target

nmap --script http-headers -p 80 target

nmap --script http-methods -p 80 target

nmap --script http-auth -p 80 target

nmap --script http-enum -p 80 target

nmap --script http-robots.txt -p 80 target

nmap --script http-shellshock -p 80 target

nmap --script http-sql-injection -p 80 target

nmap --script http-csrf -p 80 target

nmap --script http-dombased-xss -p 80 target

nmap --script http-wordpress-enum -p 80 target

nmap --script http-git -p 80 target
```

---

#### SMB

```bash
nmap --script smb-os-discovery -p 445 target

nmap --script smb-enum-shares -p 445 target

nmap --script smb-enum-users -p 445 target

nmap --script smb-security-mode -p 445 target

nmap --script smb-vuln-ms17-010 -p 445 target

nmap --script smb-vuln-ms08-067 -p 445 target

nmap --script smb-vuln-ms10-054 -p 445 target

nmap --script smb-brute -p 445 target

nmap --script "smb-vuln-*" -p 445 target
```

---

#### FTP

```bash
nmap --script ftp-anon -p 21 target

nmap --script ftp-bounce -p 21 target

nmap --script ftp-brute -p 21 target

nmap --script ftp-syst -p 21 target
```

---

#### SSH

```bash
nmap --script ssh-auth-methods -p 22 target

nmap --script ssh-hostkey -p 22 target

nmap --script ssh-brute -p 22 target

nmap --script ssh2-enum-algos -p 22 target
```

---

#### DNS

```bash
nmap --script dns-zone-transfer \
--script-args dns-zone-transfer.domain=target.com target

nmap --script dns-brute target

nmap --script dns-recursion target

nmap --script dns-cache-snoop target
```

---

#### SMTP

```bash
nmap --script smtp-enum-users -p 25 target

nmap --script smtp-open-relay -p 25 target

nmap --script smtp-commands -p 25 target
```

---

#### MySQL / MSSQL

```bash
nmap --script mysql-info -p 3306 target

nmap --script mysql-empty-password -p 3306 target

nmap --script mysql-databases -p 3306 target

nmap --script ms-sql-info -p 1433 target

nmap --script ms-sql-empty-password -p 1433 target
```

---

#### RDP

```bash
nmap --script rdp-enum-encryption -p 3389 target

nmap --script rdp-vuln-ms12-020 -p 3389 target
```

---

#### SNMP

```bash
nmap -sU --script snmp-info -p 161 target

nmap -sU --script snmp-interfaces -p 161 target

nmap -sU --script snmp-processes -p 161 target

nmap -sU --script snmp-brute -p 161 target
```

---


### 5. UDP Scanning — The Forgotten Attack Surface

Most beginners only scan TCP. Many important services also run on **UDP**, making UDP enumeration an essential part of reconnaissance.

| UDP Port | Service | Why It Matters |
|---|---|---|
| 53 | DNS | DNS enumeration and zone transfers |
| 67/68 | DHCP | DHCP infrastructure |
| 69 | TFTP | Often misconfigured and unauthenticated |
| 111 | RPC | NFS enumeration |
| 123 | NTP | Time synchronization service |
| 161/162 | SNMP | Device information and management |
| 500 | IKE/IPSec | VPN gateways |
| 1434 | MSSQL Browser | SQL Server discovery |
| 5353 | mDNS | Local network discovery |

### Why UDP Scans Are Slower

- TCP usually responds immediately with **SYN-ACK** or **RST**.
- UDP services often do **not** respond unless the packet is valid.
- Closed UDP ports typically return an ICMP "Port Unreachable" message.
- Nmap waits for timeouts, making UDP scans slower than TCP scans.

```bash
# Basic UDP scan
nmap -sU target

# Scan common UDP ports
nmap -sU -p 53,67,68,69,111,123,161,500 target

# TCP + UDP scan together
nmap -sS -sU -p T:22,80,443,445,U:53,161,67 target

# UDP version detection
nmap -sU -sV -p 161 target

# Top 100 UDP ports
nmap -sU --top-ports 100 target

# UDP with NSE
nmap -sU --script snmp-info -p 161 target
```

Example output:

```text
PORT    STATE           SERVICE
53/udp  open            domain
161/udp open|filtered   snmp
500/udp open|filtered   isakmp
```

> **Pentest Note:** `open|filtered` means Nmap could not determine whether the port is open or filtered. Additional service detection (`-sV`) can often provide more information.

---

### 6. OS Detection

Nmap identifies operating systems by analyzing responses to specially crafted packets.

```bash
# Basic OS detection
nmap -O target

# Display additional guesses
nmap -O --osscan-guess target

# Aggressive scan
nmap -A target

# Increase detection attempts
nmap -O --max-os-tries 5 target
```

Example:

```text
OS details: Linux 3.x - 4.x
OS details: Windows 10
OS details: Cisco IOS 12.x

Network Distance: 1 hop
```

#### Common Findings

| Operating System | Common Follow-Up |
|---|---|
| Windows Server | SMB enumeration |
| Windows Desktop | RDP, SMB |
| Linux | SSH, Web services |
| Cisco IOS | Network device enumeration |

> **Pentest Note:** Operating system identification combined with service versions helps prioritize further enumeration.

---

### 7. Firewall & IDS Evasion

#### Packet Fragmentation

```bash
nmap -f target

nmap -f -f target

nmap --mtu 16 target
```

---

#### Decoy Scans

```bash
nmap -D RND:10 target

nmap -D 192.168.1.5,192.168.1.6,ME target
```

---

#### Source Port Selection

```bash
nmap --source-port 53 target

nmap -g 53 target
```

---

#### Timing Controls

```bash
nmap -T0 target

nmap -T1 target

nmap --scan-delay 5s target

nmap --max-rate 10 target
```

---

#### Idle Scan

```bash
nmap -sI zombie_ip target
```

---

#### Randomization

```bash
nmap --data-length 25 target

nmap --randomize-hosts target
```

---

### 8. Version Detection

```bash
nmap -sV target

nmap -sV --version-intensity 0 target

nmap -sV --version-intensity 9 target

nmap -sV --version-all target

nmap -sV --version-light target
```

Example:

```text
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2
80/tcp   open  http    Apache 2.4.18
3306/tcp open  mysql   MySQL 5.7
```

Version detection helps identify software versions that can be researched during the vulnerability assessment phase.

---

### 9. Output Formats — Professional Reporting

Always save your scan results. Keeping organized scan reports makes later analysis much easier.

```bash
# Human-readable output
nmap -oN output.txt target

# XML output (used by many security tools)
nmap -oX output.xml target

# Greppable output
nmap -oG output.gnmap target

# Save all formats at once (recommended)
nmap -oA output target
```

Useful post-processing:

```bash
# Show only open ports
grep "open" output.gnmap

# Find hosts with port 80 open
grep "80/open" output.gnmap

# Extract live hosts
awk '/Up$/{print $2}' output.gnmap
```

---

### 10. Practical Nmap Workflow

A common reconnaissance workflow:

```bash
# Step 1 — Discover live hosts
nmap -sn 192.168.1.0/24

# Step 2 — Scan common ports
nmap --top-ports 1000 -T4 192.168.1.50

# Step 3 — Scan every TCP port
nmap -p- -T4 192.168.1.50

# Step 4 — Detect services and operating system
nmap -sV -sC -O -p 22,80,445,3306 192.168.1.50

# Step 5 — Scan important UDP ports
nmap -sU -p 53,67,69,123,161 192.168.1.50

# Step 6 — Run vulnerability detection scripts
nmap --script vuln -p 22,80,445 192.168.1.50

# Step 7 — Save everything
nmap -A -oA full_recon 192.168.1.50
```

---

### 11. Useful Nmap Commands for Labs & CTFs

```bash
# Comprehensive scan
nmap -sV -sC -A -T4 -p- <machine_ip>

# Enumerate web services
nmap --script http-enum -p 80,443,8080 <machine_ip>

# SMB enumeration
nmap --script "smb-*" -p 139,445 <machine_ip>

# SMB user enumeration
nmap --script smb-enum-users -p 445 <machine_ip>

# SMTP user enumeration
nmap --script smtp-enum-users -p 25 <machine_ip>

# SMB vulnerability checks
nmap --script smb-vuln-ms17-010 -p 445 <machine_ip>
```

---

## Quick Reference Cheatsheet

```text
Default Scripts:
nmap -sC target

Vulnerability Scripts:
nmap --script vuln target

All SMB Scripts:
nmap --script "smb-*" -p 445 target

All HTTP Scripts:
nmap --script "http-*" -p 80 target

UDP Scan:
nmap -sU -p 53,161,67,123 target

OS Detection:
nmap -O target
nmap -A target

Version Detection:
nmap -sV --version-intensity 9 target

Save Results:
nmap -oA output target

Recommended Lab Scan:
nmap -sV -sC -A -T4 -p- target
```

---

## Day 8 Submission Task

```text
Complete the Nmap learning material.

Run an advanced Nmap scan against a TryHackMe machine.

Example:

nmap -sV -sC -A -p- <machine_ip> -oA day8_scan

Save the scan results.

Take a screenshot showing:

• Open ports
• Detected services
• NSE script output

Push everything to:

100-days-pentest/
└── Day-08/

Share your progress on Discord or Telegram.
```

---

### Bonus Challenge

```text
Run:

nmap --script vuln <machine_ip>

Review the detected vulnerabilities and document your observations.
```

---

> **End of Day 8**

**Next Up:** Day 9 — Google Dorking: OSINT & Advanced Search Techniques

---
