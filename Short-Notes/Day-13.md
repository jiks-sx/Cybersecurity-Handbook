# 100-Day Pentesting Challenge
## Day 13: HTB Starting Point — Meow (First HTB Machine)

> **Platform:** HackTheBox (HTB) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| HTB Platform Setup | Learn how to use the industry-standard pentesting lab platform |
| VPN Connectivity | Connect securely to isolated HTB lab environments |
| Telnet Enumeration | Understand how to enumerate legacy services |
| Default Credentials | Learn one of the most common real-world misconfigurations |
| First HTB Methodology | Build a structured approach for attacking machines |

> This is your first Hack The Box machine. Focus on building good methodology rather than simply obtaining the flag.

---

## HTB Resources

| Resource | Purpose |
|---|---|
| Hack The Box Starting Point | Beginner machines |
| OpenVPN Configuration | Connect to HTB labs |
| HTB Academy | Guided learning modules |
| Official Walkthrough | Review after completing the machine |

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:20 | Create HTB account and connect VPN |
| 0:20 – 0:40 | Study Telnet and default credentials |
| 0:40 – 1:00 | Spawn Meow and perform reconnaissance |
| 1:00 – 1:30 | Exploit the machine |
| 1:30 – 2:00 | Document findings and complete notes |

---

## Must-Know Concepts

### 1. Hack The Box Platform

Hack The Box provides realistic machines with minimal guidance.

Platform structure:

```text
Hack The Box

├── Starting Point
│   ├── Tier 0
│   ├── Tier 1
│   └── Tier 2

├── Active Machines

├── Retired Machines

├── Academy

└── Pro Labs
```

---

### Starting Point Tiers

| Tier | Difficulty | Focus |
|---|---|---|
| Tier 0 | Beginner | Enumeration, default credentials |
| Tier 1 | Easy | Web applications, databases |
| Tier 2 | Intermediate | Multi-step exploitation |

---

### 2. HTB VPN Setup

Connect to HTB using OpenVPN.

```bash
# Install OpenVPN
sudo apt install openvpn -y

# Connect
sudo openvpn your_htb_config.ovpn

# Verify tunnel
ip a | grep tun0

# Test connectivity
ping -c 3 <machine_ip>
```

---

### Common VPN Problems

| Problem | Solution |
|---|---|
| VPN already connected | Disconnect previous session |
| No tun0 interface | Restart NetworkManager |
| Cannot reach target | Verify VPN connection |
| DNS issues | Restart VPN session |

---

### 3. Telnet

Telnet is a remote login protocol that sends all communication in plaintext.

| Feature | Telnet | SSH |
|---|---|---|
| Port | 23 | 22 |
| Encryption | No | Yes |
| Authentication | Password | Password / Keys |
| Security | Insecure | Secure |

---

### Why Pentesters Still Learn Telnet

- Legacy enterprise systems
- Network appliances
- Embedded devices
- IoT devices
- CTF challenges
- Understanding insecure protocols

---

### 4. Default Credentials

Many systems are compromised because administrators never change factory credentials.

Common examples:

| Username | Password |
|---|---|
| admin | admin |
| admin | password |
| admin | *(blank)* |
| root | *(blank)* |
| root | root |
| guest | guest |
| user | user |
| pi | raspberry |
| cisco | cisco |
| ubnt | ubnt |

---

### Credential Testing Tools

```bash
# Hydra
hydra -l admin -P passwords.txt telnet://target

hydra -l root -p "" telnet://target

# Medusa
medusa -h target -u admin -P passwords.txt -M telnet
```

---

### Best Practices

- Enumerate before exploiting.
- Record every command executed.
- Save scan results.
- Document findings.
- Never assume only one service is important.

---

### 5. Meow Methodology

Always follow a structured methodology instead of guessing.

```text
Target Spawned
        │
        ▼
Host Discovery
        │
        ▼
Port Scanning (Nmap)
        │
        ▼
Service Enumeration
        │
        ▼
Identify Telnet
        │
        ▼
Test Default Credentials
        │
        ▼
Gain Access
        │
        ▼
Search for Flag
        │
        ▼
Document Findings
```

---

### 6. Nmap Enumeration

Always begin with enumeration.

```bash
# Default scan
nmap <target_ip>

# Service detection
nmap -sV <target_ip>

# Default scripts
nmap -sC <target_ip>

# Aggressive scan
nmap -A <target_ip>

# Full port scan
nmap -p- <target_ip>

# Save results
nmap -A -oA meow_scan <target_ip>
```

---

### Example Output

```text
PORT   STATE SERVICE VERSION
23/tcp open  telnet
```

Enumeration tells you:

- Open ports
- Running services
- Service versions
- Potential attack surface

---

### 7. Telnet Enumeration

Connect to the discovered Telnet service.

```bash
telnet <target_ip>
```

Typical response:

```text
Trying <target_ip>...
Connected to <target_ip>.
Escape character is '^]'.

Ubuntu login:
```

---

### Testing Default Credentials

Common usernames:

```text
root
admin
guest
user
test
```

For the Meow machine, test the available default credentials provided during the exercise.

---

### Successful Login

After successful authentication you should receive a shell similar to:

```text
Welcome

root@Meow:~#
```

---

### Basic Enumeration

After obtaining shell access:

```bash
whoami

pwd

hostname

id

uname -a

ls

ls -la

find / -name flag.txt 2>/dev/null

find / -iname "*flag*" 2>/dev/null

cat flag.txt
```

---

### Useful Linux Commands

```bash
cd /

ls -lah

cat /etc/passwd

cat /etc/os-release

ip a

ip r

history

env
```

---

### 8. Post-Exploitation

After obtaining access, perform basic system enumeration.

Collect information such as:

| Information | Command |
|---|---|
| Current User | `whoami` |
| Hostname | `hostname` |
| User ID | `id` |
| Kernel Version | `uname -a` |
| Current Directory | `pwd` |
| Network Interfaces | `ip a` |
| Routing Table | `ip r` |
| Environment Variables | `env` |
| Operating System | `cat /etc/os-release` |

---

### Flag Discovery

Common locations:

```bash
find / -name flag.txt 2>/dev/null

find / -iname "*flag*" 2>/dev/null

locate flag.txt
```

---

### Session Flow

```text
Spawn Machine
      │
      ▼
Nmap Scan
      │
      ▼
Port 23 Found
      │
      ▼
Connect Using Telnet
      │
      ▼
Authenticate
      │
      ▼
Obtain Shell
      │
      ▼
Enumerate System
      │
      ▼
Locate Flag
      │
      ▼
Submit Flag
```

---

### Lessons Learned

- Enumeration comes before exploitation.
- Service identification determines the next step.
- Default credentials remain a common security weakness.
- Always document every command and observation.
- Build repeatable workflows rather than relying on guesswork.

---

### 9. Hack The Box Question Guide

While completing the Starting Point machine, make notes for each stage.

Record:

- Target IP Address
- Open Ports
- Running Services
- Service Versions
- Authentication Method
- Commands Executed
- Flag Location
- Lessons Learned

This habit will improve both reporting and future engagements.

---

### 10. Report Template

Use the following structure when documenting the machine.

```text
Machine:
Meow

Difficulty:
Very Easy

Platform:
Hack The Box

--------------------------------

Reconnaissance

Target IP:

Open Ports:

Services:

--------------------------------

Enumeration

Commands Executed:

Interesting Findings:

--------------------------------

Exploitation

Attack Vector:

Credentials Used:

Shell Obtained:

--------------------------------

Post Exploitation

Hostname:

Current User:

Operating System:

Flag Location:

--------------------------------

Lessons Learned

What worked?

What could be improved?

Which commands were most useful?
```

---

### 11. Common Mistakes

Avoid these beginner mistakes during labs.

| Mistake | Better Practice |
|---|---|
| Skipping enumeration | Scan first, exploit later |
| Ignoring service versions | Identify versions before searching for vulnerabilities |
| Forgetting to save scan results | Use `-oA` with Nmap |
| Running random tools | Follow a structured methodology |
| Not documenting commands | Maintain detailed notes throughout the assessment |
| Stopping after getting the flag | Continue system enumeration and understand the vulnerability |

---

### Recommended Workflow

```text
Spawn Machine
      │
      ▼
Verify VPN Connection
      │
      ▼
Run Nmap Scan
      │
      ▼
Identify Open Services
      │
      ▼
Enumerate Service
      │
      ▼
Authenticate
      │
      ▼
Gain Access
      │
      ▼
Collect Flag
      │
      ▼
Document Findings
```

---

## Quick Reference Cheatsheet

```text
VPN

sudo openvpn your_config.ovpn

Verify Tunnel

ip a | grep tun0

Connectivity Test

ping -c 3 <target_ip>

Nmap

nmap <target_ip>

nmap -sV <target_ip>

nmap -sC <target_ip>

nmap -A <target_ip>

nmap -p- <target_ip>

nmap -A -oA meow_scan <target_ip>

Telnet

telnet <target_ip>

System Enumeration

whoami

hostname

id

pwd

uname -a

ip a

ip r

env

cat /etc/os-release

Flag Search

find / -name flag.txt 2>/dev/null

find / -iname "*flag*" 2>/dev/null
```

---

## Day 13 Submission Task

```text
Complete the HTB Starting Point machine: Meow.

Document:

• Target IP Address
• Open Ports
• Running Services
• Nmap Commands
• Enumeration Process
• Authentication Method
• Shell Access
• Flag Location
• Lessons Learned

Save your notes in Markdown.

Push everything to:

100-days-pentest/
└── Day-13/

Include:

• Nmap scan output
• Terminal screenshots
• Final flag submission confirmation
```

---

### Bonus Challenge

```text
Repeat the entire machine without using the official walkthrough.

Create your own methodology checklist.

Compare your workflow with your notes and identify areas for improvement.
```

---

> **End of Day 13**

**Next Up:** **Day 14 — HTB Starting Point: Fawn**

---
