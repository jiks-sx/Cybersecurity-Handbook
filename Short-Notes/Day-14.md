# 100-Day Pentesting Challenge
## Day 14: HTB Starting Point — Fawn (FTP Enumeration)

> **Platform:** HackTheBox (HTB) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| FTP Protocol Deep Dive | FTP is widely used on servers, network devices and legacy systems |
| Anonymous FTP Access | One of the most common misconfigurations encountered during security assessments |
| FTP Active vs Passive Mode | Understand how FTP behaves through firewalls |
| FTP Enumeration Techniques | Discover files, directories and server information |
| File Transfer in Pentesting | Download and upload files during engagements |

> FTP misconfigurations such as anonymous access, weak credentials and plaintext communication are still common findings during penetration tests.

---

## HTB Access

| Resource | Link |
|---|---|
| HTB Starting Point | https://app.hackthebox.com/starting-point |
| HTB Academy — FTP | https://academy.hackthebox.com/module/18 |
| HTB Academy — Network Enumeration | https://academy.hackthebox.com/module/19 |

> HTB Starting Point Tier 0 is available for free.

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:10 | Connect HTB VPN and spawn the Fawn machine |
| 0:10 – 0:30 | Study FTP protocol, ports and communication modes |
| 0:30 – 0:55 | Perform reconnaissance using Nmap |
| 0:55 – 1:15 | Study anonymous FTP and enumeration techniques |
| 1:15 – 1:40 | Access the machine and retrieve the flag |
| 1:40 – 2:00 | Document methodology and findings |

---

## Must-Know Concepts

### 1. FTP — File Transfer Protocol

FTP is a TCP-based protocol designed for transferring files between a client and a server.

Unlike many other protocols, FTP uses two separate TCP connections.

---

### FTP Channels

```text
Control Channel
Port 21

Responsible for:

• Authentication
• Commands
• Session management

--------------------------------

Data Channel

Active Mode:
Port 20

Passive Mode:
Random high-numbered port

Responsible for:

• File transfers
• Directory listings
```

---

### FTP vs FTPS vs SFTP

| Protocol | Encryption | Port | Notes |
|---|---|---|---|
| FTP | None | 21 | Plaintext communication |
| FTPS | SSL/TLS | 21 / 990 | FTP protected with TLS |
| SFTP | SSH | 22 | File transfer over SSH |

---

### Why FTP Matters

FTP traffic is transmitted without encryption.

An attacker with network visibility can capture:

- Usernames
- Passwords
- File contents
- Directory listings

---

### 2. Active vs Passive FTP

Understanding these modes is important because firewall behavior differs.

---

### Active Mode

```text
Client
   │
   ├──── Control Connection (21) ───► Server
   │
   ◄──── Data Connection (20)
```

Characteristics:

- Client initiates control connection.
- Server initiates data connection.
- Often blocked by client firewalls.

---

### Passive Mode

```text
Client
   │
   ├──── Control Connection (21) ───► Server
   │
   ├──── Data Connection (Random Port) ─► Server
```

Characteristics:

- Client initiates both connections.
- Compatible with NAT and firewalls.
- Default mode in most modern FTP clients.

---

### Enable Passive Mode

```bash
ftp -p <target_ip>

ftp> passive

lftp <target_ip>
```

---

### 3. FTP Client Commands

Connect:

```bash
ftp <target_ip>

ftp -p <target_ip>
```

Navigation:

```bash
ls

ls -la

pwd

cd directory

cd ..
```

Download Files:

```bash
get file.txt

mget *

mget *.txt
```

Upload Files:

```bash
put file.txt

mput *.txt
```

Transfer Modes:

```bash
binary

ascii
```

Information Commands:

```bash
status

passive

verbose

quote SYST

quote HELP

quote FEAT
```

Disconnect:

```bash
quit

bye

exit
```

---

### 4. Anonymous FTP

Anonymous FTP allows users to access a server without having a dedicated account.

Typical credentials:

```text
Username:
anonymous

Password:
(blank)
```

Some servers also accept an email address as the password.

---

### Checking Anonymous FTP

Manual connection:

```bash
ftp <target_ip>

Name:
anonymous

Password:
(press Enter)
```

Nmap:

```bash
nmap --script ftp-anon -p 21 <target_ip>
```

Netcat:

```bash
nc -w 3 <target_ip> 21
```

curl:

```bash
curl ftp://anonymous:@<target_ip>/
```

wget:

```bash
wget -r ftp://anonymous:@<target_ip>/
```

lftp:

```bash
lftp -u anonymous, <target_ip>
```

---

### After Anonymous Login

List files:

```bash
ls

ls -la

ls -R
```

Download files:

```bash
mget *

mget *.txt

mget *.conf

mget *.php

mget *.sql
```

Check write access:

```bash
put test.txt
```

If uploads succeed, the server allows anonymous write access and should be documented as a security finding during an authorized assessment.

---

### 5. The Fawn Methodology

Follow a structured workflow instead of relying on guesswork.

```text
Spawn Machine
        │
        ▼
Verify VPN Connectivity
        │
        ▼
Run Nmap Scan
        │
        ▼
Identify FTP Service
        │
        ▼
Check Anonymous Access
        │
        ▼
Connect to FTP
        │
        ▼
Enumerate Files
        │
        ▼
Download Flag
        │
        ▼
Document Findings
```

---

### 6. Nmap Enumeration

Always begin with reconnaissance.

```bash
# Default scan
nmap <target_ip>

# Version detection
nmap -sV <target_ip>

# Default NSE scripts
nmap -sC <target_ip>

# Aggressive scan
nmap -A <target_ip>

# Save results
nmap -A -oA fawn_scan <target_ip>
```

---

### Example Output

```text
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3

| ftp-anon:
| Anonymous FTP login allowed (FTP code 230)

| ftp-syst:
| vsFTPd 3.0.3
```

Important findings:

- FTP is running.
- Anonymous login is allowed.
- Server version is identified.
- NSE scripts confirm anonymous access.

---

### 7. Connect to FTP

```bash
ftp <target_ip>
```

Example session:

```text
Connected to <target_ip>

220 (vsFTPd 3.0.3)

Name:
anonymous

Password:
(Press Enter)

230 Login successful
```

---

### Basic Enumeration

```bash
ls

ls -la

pwd

status

quote SYST

quote FEAT
```

---

### Download the Flag

```bash
get flag.txt

quit

cat flag.txt
```

---

### 8. FTP Enumeration

Never stop after downloading a single file.

Continue enumerating.

```bash
ls -la

ls -R

pwd

cd /

cd backup

cd uploads

cd public
```

Download everything useful:

```bash
mget *

mget *.txt

mget *.conf

mget *.php

mget *.sql

mget *.bak
```

---

### Banner Grabbing

Netcat:

```bash
nc -w 5 <target_ip> 21
```

Telnet:

```bash
telnet <target_ip> 21
```

curl:

```bash
curl -v ftp://anonymous:@<target_ip>/

curl ftp://anonymous:@<target_ip>/ --list-only
```

wget:

```bash
wget -r ftp://anonymous:@<target_ip>/
```

lftp:

```bash
lftp ftp://anonymous:@<target_ip>

ls -la

find

mirror /remote/path /local/path
```

---

### 9. Real-World FTP Attack Scenarios

#### Anonymous FTP

Search for:

- Configuration files
- Database backups
- Password files
- Source code
- Archives

Example:

```bash
grep -Ri "password" .

grep -Ri "secret" .

grep -Ri "token" .
```

---

#### Writable FTP

Test upload permission:

```bash
echo "test" > test.txt

put test.txt
```

If uploads succeed, document the finding.

---

#### FTP Brute Force

Hydra:

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt ftp://<target_ip>
```

Multiple users:

```bash
hydra -L users.txt -P /usr/share/wordlists/rockyou.txt ftp://<target_ip>
```

Nmap:

```bash
nmap --script ftp-brute -p 21 <target_ip>
```

---

#### FTP NSE Scripts

```bash
nmap --script "ftp-*" -p 21 <target_ip>

nmap --script ftp-anon -p 21 <target_ip>

nmap --script ftp-syst -p 21 <target_ip>
```

---

### 10. vsftpd

Fawn uses **vsftpd**.

Configuration file:

```bash
cat /etc/vsftpd.conf
```

Common options:

```text
anonymous_enable=YES

anon_upload_enable=YES

write_enable=YES

local_enable=YES

chroot_local_user=YES
```

Important observations:

- `anonymous_enable=YES` allows anonymous authentication.
- `anon_upload_enable=YES` allows anonymous uploads.
- Upload capability should always be documented during an authorized assessment.

---

### 11. Binary vs ASCII Mode

Text files:

```bash
ascii
```

Binary files:

```bash
binary
```

Use **binary mode** when transferring:

- ZIP archives
- Images
- Executables
- PDFs
- Payloads
- Any non-text file

Using ASCII mode for binary files may corrupt transferred data.

---

### Lessons Learned

- Enumeration always comes before exploitation.
- FTP versions should be documented.
- Anonymous access must always be tested.
- Download all accessible files before moving on.
- Check whether uploads are permitted.
- Save all scan results and commands.

---

### 12. HTB Guided Questions

While completing the Fawn machine, document each stage of your assessment.

Record the following information:

- Target IP Address
- Open Ports
- Running Services
- Service Version
- Authentication Method
- Anonymous Login Status
- Files Discovered
- Flag Location
- Commands Executed
- Lessons Learned

Keeping structured notes will make future reporting much easier.

---

### 13. Common FTP Response Codes

Understanding FTP response codes helps identify the current state of the server.

| Code | Meaning |
|------|---------|
| 150 | File status okay, opening data connection |
| 200 | Command successful |
| 220 | FTP service ready |
| 221 | Closing connection |
| 226 | File transfer completed |
| 227 | Entering Passive Mode |
| 230 | Login successful |
| 331 | Username accepted, password required |
| 425 | Cannot open data connection |
| 426 | Connection closed during transfer |
| 530 | Login failed |
| 550 | File unavailable or permission denied |

---

### 14. Meow vs Fawn

| Feature | Meow | Fawn |
|---------|------|-------|
| Service | Telnet | FTP |
| Port | 23 | 21 |
| Attack Method | Default Credentials | Anonymous Login |
| Authentication | Username Login | Anonymous User |
| Goal | Obtain Shell | Download Files |
| Main Weakness | Weak Credentials | Anonymous FTP Enabled |

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
Identify FTP Service
      │
      ▼
Check Anonymous Access
      │
      ▼
Login to FTP
      │
      ▼
Enumerate Directories
      │
      ▼
Download Files
      │
      ▼
Retrieve Flag
      │
      ▼
Document Findings
```

---

## Quick Reference Cheatsheet

```text
VPN

sudo openvpn your_config.ovpn

Verify Connection

ip a | grep tun0

ping -c 3 <target_ip>

Nmap

nmap <target_ip>

nmap -sV <target_ip>

nmap -sC <target_ip>

nmap -A <target_ip>

nmap --script ftp-anon -p 21 <target_ip>

FTP Login

ftp <target_ip>

Username:
anonymous

Password:
(blank)

FTP Enumeration

ls

ls -la

pwd

status

quote SYST

quote FEAT

Download

get flag.txt

mget *

Upload Test

put test.txt

Banner Grab

nc -w 5 <target_ip> 21

telnet <target_ip> 21

curl ftp://anonymous:@<target_ip>/

wget -r ftp://anonymous:@<target_ip>/

FTP Scripts

nmap --script ftp-anon -p 21 <target_ip>

nmap --script ftp-syst -p 21 <target_ip>

nmap --script ftp-brute -p 21 <target_ip>
```

---

## Day 14 Submission Task

```text
Complete the HTB Starting Point machine: Fawn.

Document:

• Target IP Address
• Open Ports
• Running Services
• FTP Version
• Anonymous Login Status
• Files Downloaded
• Flag Location
• Commands Executed
• Lessons Learned

Save your notes in Markdown.

Push everything to:

100-days-pentest/
└── Day-14/

Include:

• Nmap scan output
• FTP session screenshots
• Flag submission confirmation
```

---

### Bonus Challenges

```text
1. Enumerate the FTP server using:

• ftp
• curl
• wget
• lftp
• netcat

Compare the output from each tool.

--------------------------------

2. Research:

• Active FTP
• Passive FTP

Explain:

• How each mode works
• Which ports are used
• Why Passive Mode is preferred today
```

---

> **End of Day 14**

**Next Up:** **Day 15 — HTB Starting Point: Dancing (SMB Enumeration)**

---
