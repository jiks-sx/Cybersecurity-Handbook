# 100-Day Pentesting Challenge
## Day 6: Intro to Networking — Ports, Protocols & Wireshark

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** ~1 Hour

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| Ports & Protocols | Every open port is a potential entry point |
| Network Topologies | Understand how networks are physically laid out |
| Wireshark | Analyze real network traffic |

> **Ports are doors. Protocols are languages. Wireshark lets you inspect network traffic.**
> Understanding packets helps you analyze protocols, troubleshoot issues, and perform security assessments.

---

## THM Rooms to Complete (In Order)

| # | Room | Link | Focus |
|---|---|---|---|
| 1 | Network Services | https://tryhackme.com/room/networkservices | SMB, Telnet, FTP enumeration |
| 2 | Network Services 2 | https://tryhackme.com/room/networkservices2 | NFS, SMTP, MySQL enumeration |

> All rooms are available on TryHackMe.

---

## 1-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:10 | Read the theory below (Ports + Wireshark quick reference) |
| 0:10 – 0:40 | Complete THM Rooms 1 & 2 |
| 0:40 – 1:00 | Review the cheatsheet and update GitHub |

---

## Must-Know Concepts

### 1. Port Ranges

```text
Well-Known Ports:    0 – 1023      Standard services
Registered Ports:    1024 – 49151  Application-specific
Dynamic Ports:       49152 – 65535 Temporary client ports
```

---

### 2. Common Ports & Protocols

| Port | Protocol | Service | Typical Enumeration |
|---|---|---|---|
| 21 | TCP | FTP | Check anonymous access and banner |
| 22 | TCP | SSH | Identify version and authentication methods |
| 23 | TCP | Telnet | Check for plaintext access |
| 25 | TCP | SMTP | Banner grabbing and user enumeration |
| 53 | TCP/UDP | DNS | Record lookup and DNS enumeration |
| 80 | TCP | HTTP | Web application enumeration |
| 110 | TCP | POP3 | Mail service enumeration |
| 139/445 | TCP | SMB | Share and user enumeration |
| 143 | TCP | IMAP | Mail service enumeration |
| 443 | TCP | HTTPS | HTTPS application testing |
| 3306 | TCP | MySQL | Database enumeration |
| 3389 | TCP | RDP | Remote Desktop identification |
| 5900 | TCP | VNC | Remote desktop service |
| 8080/8443 | TCP | Alternate HTTP/HTTPS | Web interfaces and admin panels |

---

### 3. Service Enumeration Commands

#### FTP (Port 21)

```bash
ftp <target_ip>
nmap --script ftp-anon <target>
nmap --script ftp-brute <target>
```

#### SMB (Port 445)

```bash
smbclient -L //<target_ip>
smbclient //<target_ip>/share
enum4linux -a <target_ip>
nmap --script smb-enum-shares <target>
nmap --script smb-vuln-ms17-010 <target>
```

#### SMTP (Port 25)

```bash
nc <target> 25
nmap --script smtp-enum-users <target>
```

#### MySQL (Port 3306)

```bash
mysql -u root -h <target_ip>
nmap --script mysql-info <target>
```

---

### 4. Wireshark Essentials

#### Starting a Capture

```text
1. Open Wireshark
2. Select a network interface
3. Click Start Capture
4. Stop the capture when finished
5. Save as a .pcap file
```

#### Useful Display Filters

| Filter | Description |
|---|---|
| `http` | HTTP traffic |
| `dns` | DNS traffic |
| `tcp` | TCP packets |
| `udp` | UDP packets |
| `ftp` | FTP traffic |
| `smtp` | SMTP traffic |
| `ip.addr == 192.168.1.10` | Traffic to or from an IP |
| `ip.src == 192.168.1.10` | Source IP |
| `ip.dst == 192.168.1.10` | Destination IP |
| `tcp.port == 21` | FTP traffic |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | SYN packets |
| `http.request.method == "POST"` | POST requests |
| `frame contains "password"` | Packets containing a string |
| `http.response.code == 200` | Successful HTTP responses |

#### Useful Features

- **Follow TCP Stream** — View the complete TCP conversation.
- **Statistics → Protocol Hierarchy** — Display protocol distribution.
- **Statistics → Conversations** — Show communication between hosts.
- **Export Objects → HTTP** — Extract transferred files.
- **Find Packet (Ctrl+F)** — Search packet contents.

---

### 5. Reading a Packet

```text
Frame
├── Ethernet II
│     Source MAC
│     Destination MAC
├── Internet Protocol (IPv4)
│     Source IP
│     Destination IP
├── TCP
│     Source Port
│     Destination Port
│     TCP Flags
└── HTTP
      Request / Response Data
```

> Understanding packet layers helps correlate OSI and TCP/IP concepts with real network traffic.

---

### 6. Network Topologies

| Topology | Shape | Advantages | Disadvantages | Security Consideration |
|---|---|---|---|---|
| Bus | Single cable | Simple | Single cable failure affects all | Shared medium |
| Ring | Circular | Predictable flow | Single break impacts communication | Sequential traffic |
| Star | Central switch | Easy management | Central device dependency | Common enterprise design |
| Mesh | Fully connected | High redundancy | Expensive | Multiple communication paths |
| Tree | Hierarchical | Scalable | Root dependency | Common in large organizations |

---

## Quick Reference Cheatsheet

```text
Common Ports:
21   FTP
22   SSH
23   Telnet
25   SMTP
53   DNS
80   HTTP
139/445 SMB
443  HTTPS
3306 MySQL
3389 RDP

FTP:
ftp <ip>

SMB:
enum4linux -a <ip>
smbclient -L //<ip>

Useful Wireshark Filters:
http
dns
tcp
ip.addr == x.x.x.x
frame contains "password"

Follow TCP Stream:
Right-click packet → Follow → TCP Stream
```

---

## Day 6 Submission Task

```text
Complete both TryHackMe rooms

Take screenshots showing room completion

(Optional)
Capture network traffic in Wireshark and save a screenshot

Push your work to:
100-days-pentest/Day-06/

Share your progress on Discord or Telegram
```

---

> **End of Day 6**

**Next Up:** Day 7 — Nmap: Mastering the Most Widely Used Reconnaissance Tool

---
