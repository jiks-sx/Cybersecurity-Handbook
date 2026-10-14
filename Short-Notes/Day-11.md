# 100-Day Pentesting Challenge
## Day 11: Active Reconnaissance — traceroute, ping & curl

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| `ping` & ICMP | Discover live hosts, measure latency, and understand firewall behavior |
| `traceroute` / `tracert` | Map the network path and identify intermediate routers |
| `curl` | Manually craft HTTP requests for web reconnaissance |
| `wget` | Download files and mirror websites |
| Active Recon Tools | Learn practical reconnaissance techniques used during engagements |

> Active reconnaissance involves direct interaction with the target. Every request can be logged, so always ensure you have proper authorization before performing active reconnaissance.

---

## THM Rooms to Complete (In Order)

| # | Room | Link | Focus |
|---|---|---|---|
| 1 | Active Reconnaissance | https://tryhackme.com/room/activerecon | ping, traceroute, telnet, curl |

> All rooms are available on TryHackMe.

---

## 1.5-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:15 | Study ICMP, ping, and traceroute concepts |
| 0:15 – 0:40 | Complete the TryHackMe room |
| 0:40 – 1:30 | Practice curl and active reconnaissance techniques |

---

## Must-Know Concepts

### 1. `ping` — Host Discovery & ICMP Analysis

`ping` uses **ICMP Echo Request** packets and waits for **ICMP Echo Reply** packets.

It helps determine:

- Whether a host is reachable
- Network latency
- Packet loss
- Approximate operating system (using TTL)
- Firewall behavior

---

### Basic Usage

```bash
# Linux
ping 8.8.8.8

ping -c 4 8.8.8.8

ping -c 1 192.168.1.1

ping -i 0.2 target.com

ping -s 1000 target.com

ping -t 64 target.com

ping -W 1 target.com

ping -f target.com

ping -q target.com

# Windows
ping 8.8.8.8

ping -t 8.8.8.8

ping -n 10 8.8.8.8

ping -l 1000 8.8.8.8

ping -i 5 8.8.8.8
```

---

### Understanding Ping Output

```text
PING 8.8.8.8 (8.8.8.8)

64 bytes from 8.8.8.8:
icmp_seq=1 ttl=118 time=14.2 ms

64 bytes from 8.8.8.8:
icmp_seq=2 ttl=118 time=13.9 ms

--- Ping Statistics ---

4 packets transmitted

4 received

0% packet loss

Average latency: 14 ms
```

---

### Output Fields

| Field | Description |
|---|---|
| `ttl` | Remaining Time To Live value |
| `time` | Round-trip latency |
| `icmp_seq` | Packet sequence number |
| Packet Loss | Indicates dropped packets |

---

### TTL Fingerprinting

| Default TTL | Likely Operating System |
|---|---|
| 64 | Linux / macOS |
| 128 | Windows |
| 255 | Cisco IOS / BSD |

Example:

```
Received TTL = 118

128 - 118 = 10 hops

Likely Windows host approximately 10 hops away.
```

---

### Ping Sweep

```bash
# Bash

for i in {1..254}; do

ping -c 1 -W 1 192.168.1.$i >/dev/null 2>&1 && \
echo "192.168.1.$i is alive"

done

# fping

fping -a -g 192.168.1.0/24

# Nmap

nmap -sn 192.168.1.0/24

# Multiple discovery techniques

nmap -sn -PE -PS22,80,443 -PA80 -PU53 192.168.1.0/24
```

---

### ICMP Response Types

| ICMP Response | Meaning |
|---|---|
| Echo Reply | Host is reachable |
| Host Unreachable | No route to host |
| Port Unreachable | Common during UDP scans |
| Administratively Prohibited | Firewall blocked the request |
| Time Exceeded | TTL expired (used by traceroute) |
| Timeout | Host unavailable or ICMP filtered |

---

### 2. `traceroute` / `tracert`

`traceroute` identifies every router between your system and the target.

Each packet is sent with an increasing TTL value.

Every router decreases the TTL by one.

When TTL reaches zero, that router responds with an ICMP **Time Exceeded** message.

This allows you to map the route taken by packets.

```text
You

↓

Router 1

↓

ISP Gateway

↓

Backbone Router

↓

Target
```

---

### Basic Commands

```bash
# Linux

traceroute google.com

traceroute -I google.com

traceroute -T -p 80 google.com

traceroute -T -p 443 google.com

traceroute -n google.com

traceroute -m 30 google.com

# Windows

tracert google.com

tracert -d google.com

tracert -h 30 google.com

tracert -w 1000 google.com
```

---

### Reading traceroute Output

| Output | Meaning |
|---|---|
| Router IP | Router responded successfully |
| `* * *` | No response from router |
| High latency | Congestion or long-distance routing |
| `!H` | Host unreachable |
| `!N` | Network unreachable |
| `!P` | Port unreachable |

> **Pentest Note:** Silent hops (`* * *`) often indicate filtering or routers configured not to reply to traceroute packets.

---

### 3. `curl` — HTTP Requests from the Terminal

`curl` is one of the most important tools for web reconnaissance and penetration testing.

It allows you to:

- Send custom HTTP requests
- Test APIs
- Inspect HTTP headers
- Upload and download files
- Work with cookies and authentication
- Route requests through Burp Suite

---

### Basic Requests

```bash
# Basic GET request
curl http://target.com

# Verbose output
curl -v http://target.com

# Headers only
curl -I http://target.com

curl -I https://target.com

# Follow redirects
curl -L http://target.com

# Silent mode
curl -s http://target.com

# Save output
curl -o output.html http://target.com

# Download using original filename
curl -O http://target.com/file.txt
```

---

### HTTP Methods

```bash
# POST request
curl -X POST http://target.com/login \
-d "username=admin&password=admin123"

# JSON POST
curl -X POST http://target.com/api/login \
-H "Content-Type: application/json" \
-d '{"username":"admin","password":"admin123"}'

# PUT
curl -X PUT http://target.com/api/user/1 \
-H "Content-Type: application/json" \
-d '{"name":"admin"}'

# DELETE
curl -X DELETE http://target.com/api/user/1

# PATCH
curl -X PATCH http://target.com/api/user/1 \
-d '{"role":"admin"}'

# OPTIONS
curl -X OPTIONS http://target.com -v
```

---

### Custom Headers

```bash
# Custom header
curl -H "X-Forwarded-For: 127.0.0.1" http://target.com

# User-Agent
curl -A "Mozilla/5.0" http://target.com

curl -A "Googlebot/2.1" http://target.com

# Multiple headers
curl \
-H "Authorization: Bearer TOKEN" \
-H "Content-Type: application/json" \
http://target.com/api

# Cookies
curl -b "sessionid=abc123" http://target.com

# Cookies from file
curl -b cookies.txt http://target.com

# Save cookies
curl -c cookies.txt http://target.com
```

---

### Authentication

```bash
# Basic Authentication
curl -u admin:password http://target.com/admin

# Bearer Token
curl -H "Authorization: Bearer TOKEN" \
http://target.com/api

# API Key
curl -H "X-API-Key: APIKEY" \
http://target.com/api
```

---

### SSL/TLS

```bash
# Ignore certificate validation
curl -k https://target.com

curl --insecure https://target.com

# Display certificate information
curl -v --head https://target.com

# Maximum TLS version
curl --tls-max 1.2 https://target.com
```

---

### Using Burp Suite as a Proxy

```bash
# Send traffic through Burp
curl -x http://127.0.0.1:8080 \
-k http://target.com

# Environment variables
export http_proxy=http://127.0.0.1:8080
export https_proxy=http://127.0.0.1:8080

curl https://target.com
```

---

### Common Web Testing Examples

```bash
# Local File Inclusion
curl "http://target.com/page.php?file=../../../../etc/passwd"

# Basic SQL Injection
curl "http://target.com/item.php?id=1'"

# Reflected XSS
curl "http://target.com/search.php?q=<script>alert(1)</script>"

# SSRF
curl "http://target.com/fetch.php?url=http://169.254.169.254/latest/meta-data/"

# Response headers
curl -I http://target.com

# CORS
curl \
-H "Origin: https://evil.com" \
-I http://target.com

# Allowed methods
curl -X OPTIONS http://target.com -v
```

---

### 4. `wget`

`wget` is commonly used to download files, mirror websites, and retrieve resources from remote servers.

```bash
# Download a file
wget http://target.com/file.txt

# Silent mode
wget -q http://target.com/file.txt

# Save to a directory
wget -P /tmp http://target.com/file.txt

# Mirror a website
wget -r http://target.com

# Recursive download depth
wget -r -l 2 http://target.com

# Download only PDF and DOCX files
wget -r -A "*.pdf,*.docx" http://target.com

# Resume interrupted download
wget -c http://target.com/file.zip

# Spider mode
wget --spider -r http://target.com

# Custom User-Agent
wget -U "Mozilla/5.0" http://target.com
```

---

### 5. `telnet`

Although largely replaced by SSH, `telnet` remains useful for manual service interaction and banner grabbing.

```bash
# Connect to HTTP
telnet target.com 80

# SMTP
telnet target.com 25

# FTP
telnet target.com 21

# POP3
telnet target.com 110
```

---

### Manual HTTP Request

```text
GET / HTTP/1.0
Host: target.com

```

---

### Manual SMTP Session

```text
HELO attacker.com
MAIL FROM:<test@test.com>
RCPT TO:<admin@target.com>
QUIT
```

---

### 6. `netcat` (`nc`)

Netcat is a flexible networking utility for connecting to services, checking ports, transferring data, and banner grabbing.

```bash
# Connect to TCP ports
nc target.com 80

nc target.com 22

nc target.com 25

# Manual HTTP request
echo -e "GET / HTTP/1.0\r\nHost: target.com\r\n\r\n" | nc target.com 80

# Port scan
nc -zv target.com 80

nc -zv target.com 1-1000

# Listen
nc -lvnp 4444

# Banner grabbing
nc -w 1 target.com 25

# UDP
nc -u target.com 53
```

---

### 7. Content Discovery

Finding hidden files and directories is one of the first steps during web application reconnaissance.

Always begin with common files that are intentionally exposed.

---

### `robots.txt`

```bash
# View robots.txt
curl http://target.com/robots.txt

# View sitemap
curl http://target.com/sitemap.xml
```

Common discoveries:

```text
Disallow: /admin
Disallow: /backup
Disallow: /private
Disallow: /test
```

These paths should be investigated during an authorized assessment.

---

### Directory Enumeration

#### Gobuster

```bash
# Basic scan
gobuster dir \
-u http://target.com \
-w /usr/share/wordlists/dirb/common.txt

# Medium wordlist
gobuster dir \
-u http://target.com \
-w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# Search specific extensions
gobuster dir \
-u http://target.com \
-w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt \
-x php,html,txt,bak

# Ignore SSL certificate
gobuster dir \
-u https://target.com \
-k \
-w /usr/share/wordlists/dirb/common.txt
```

---

#### FFUF

```bash
# Basic directory fuzzing
ffuf \
-w /usr/share/wordlists/dirb/common.txt \
-u http://target.com/FUZZ

# File extensions
ffuf \
-w /usr/share/wordlists/dirb/common.txt \
-u http://target.com/FUZZ \
-e .php,.txt,.html

# Virtual Host fuzzing
ffuf \
-w subdomains.txt \
-u http://target.com \
-H "Host: FUZZ.target.com"

# Parameter fuzzing
ffuf \
-w parameters.txt \
-u "http://target.com/index.php?FUZZ=test"
```

---

#### Dirb

```bash
dirb http://target.com

dirb http://target.com \
/usr/share/wordlists/dirb/big.txt
```

---

#### Feroxbuster

```bash
feroxbuster \
-u http://target.com

feroxbuster \
-u http://target.com \
-w /usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt
```

---

### Useful Wordlists

```bash
/usr/share/wordlists/

/usr/share/wordlists/dirb/

/usr/share/wordlists/dirbuster/

/usr/share/seclists/Discovery/Web-Content/
```

Install SecLists if needed:

```bash
sudo apt update

sudo apt install seclists -y
```

---

### 8. Web Application Fingerprinting

Identify the technologies running on the target before beginning further enumeration.

---

#### HTTP Headers

```bash
curl -I http://target.com

curl -I https://target.com
```

---

#### WhatWeb

```bash
whatweb http://target.com

whatweb -a 3 http://target.com
```

---

#### Nikto

```bash
nikto -h http://target.com

nikto -h http://target.com -p 8080
```

---

#### Page Source Analysis

```bash
curl -s http://target.com \
| grep -Ei "generator|framework|jquery|wordpress|version"
```

---

#### Check Common Admin Paths

```bash
for path in \
admin \
administrator \
wp-admin \
phpmyadmin \
login \
control \
panel \
manager
do

code=$(curl -s \
-o /dev/null \
-w "%{http_code}" \
http://target.com/$path)

echo "$path -> $code"

done
```

---

### 9. HTTP Header Analysis

```bash
curl -I http://target.com
```

Common headers:

| Header | Example | Why It Matters |
|---|---|---|
| `Server` | Apache/2.4.18 | Service version |
| `X-Powered-By` | PHP/7.2 | Backend technology |
| `Set-Cookie` | PHPSESSID | Session management |
| `Strict-Transport-Security` | Present | HTTPS enforcement |
| `Content-Security-Policy` | Present | Browser security policy |
| `X-Frame-Options` | DENY | Clickjacking protection |
| `Access-Control-Allow-Origin` | * | CORS configuration |

---

### 10. Active Recon Workflow

```bash
TARGET="target.com"

IP=$(dig +short $TARGET)

echo "[*] Ping"
ping -c 4 $TARGET

echo "[*] Traceroute"
traceroute -n $TARGET

echo "[*] Nmap"
nmap -sV -sC -T4 $IP

echo "[*] Headers"
curl -I http://$TARGET

echo "[*] robots.txt"
curl http://$TARGET/robots.txt

echo "[*] Technology"
whatweb http://$TARGET

echo "[*] Directories"
gobuster dir \
-u http://$TARGET \
-w /usr/share/wordlists/dirb/common.txt
```

---

## Quick Reference Cheatsheet

```text
Ping:
ping -c 4 target

Ping Sweep:
nmap -sn 192.168.1.0/24

Traceroute:
traceroute -n target

Headers:
curl -I http://target

Verbose Request:
curl -v http://target

Ignore SSL:
curl -k https://target

Proxy Through Burp:
curl -x http://127.0.0.1:8080 http://target

robots.txt:
curl http://target/robots.txt

Gobuster:
gobuster dir -u http://target -w /usr/share/wordlists/dirb/common.txt

FFUF:
ffuf -w /usr/share/wordlists/dirb/common.txt -u http://target/FUZZ

Banner Grab:
nc target 80

Technology:
whatweb http://target

Nikto:
nikto -h http://target
```

---

## Day 11 Submission Task

```text
Complete the TryHackMe room.

Practice:

• ping
• traceroute
• curl
• robots.txt analysis
• gobuster directory enumeration

Document:

• Ping results
• Traceroute output
• HTTP response headers
• Web technologies identified
• Interesting directories discovered

Push everything to:

100-days-pentest/
└── Day-11/

Share your progress on Discord or Telegram.
```

---

### Bonus Challenge

```text
Route curl through Burp Suite.

Capture the request and response.

Identify:

• Server software
• Backend technology
• Cookies
• Security headers

Document your observations.
```

---

> **End of Day 11**

**Next Up:** Day 12 — OWASP Top 10: SQL Injection

---
