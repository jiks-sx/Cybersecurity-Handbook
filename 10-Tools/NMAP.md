
> ** Nmap**

# Nmap - Introduction

---

# Introduction

When performing a security assessment, the first step is to gather information about the target.

The more information you have about a target system or network, the easier it becomes to identify possible attack paths.

This information gathering process is called **Enumeration**.

Before attempting any exploitation, it is important to discover what services are running on the target machine.

---

# Why is Enumeration Important?

Enumeration helps build a map of the target.

This map shows:

- Active hosts
- Open ports
- Running services
- Network structure

Without this information, it is difficult to know where to begin an assessment.

---

# What is Port Scanning?

Port scanning is the process of checking which ports on a target machine are open.

A computer running a network service opens a **port** so that it can receive incoming connections.

By scanning these ports, we can identify which services are available.

---

# What is a Port?

A port is a networking endpoint used for communication between devices.

Ports allow multiple services to run on the same computer at the same time.

Every computer has **65,535** available ports.

Some ports are commonly associated with specific services.

Examples include:

| Port | Service |
|------|---------|
| 80 | HTTP |
| 443 | HTTPS |
| 139 | NetBIOS |
| 445 | SMB |

The room also notes that these services may use different ports in some situations.

---

# How Network Communication Works

Communication between two systems happens through ports.

The client opens a random high-numbered port.

The server listens on a specific port.

The connection is then established between these two ports.

Example:

```text
Client
Port 49534
      │
      │ Connection
      ▼
Server
Port 443 (HTTPS)
```

---

# Why Do We Scan Ports?

Before attacking a target, we need to know:

- Which ports are open.
- Which services are running.
- Which ports are closed.
- Which ports are filtered.

Without this information, selecting the correct attack path becomes difficult.

---

# What is Nmap?

Nmap is a network scanning tool used for initial enumeration.

It scans the target's ports and identifies whether they are:

- Open
- Closed
- Filtered

After identifying open ports, Nmap can also help identify the services running on those ports.

The room describes Nmap as the **industry standard** for port scanning.

---

# How Nmap Works

Nmap scans ports one by one.

For each port, it sends packets and observes the response.

Based on the response, Nmap determines the port state.

The possible results are:

## Open

The target accepts the connection.

A service is listening on the port.

---

## Closed

The target responds, but no service is running on the port.

---

## Filtered

The target does not respond, or the traffic is blocked by a firewall.

---

# Nmap Scanning Flow

```text
Target Machine
        │
        ▼
Select Port
        │
        ▼
Send Packet
        │
        ▼
Receive Response
        │
        ├── Open
        ├── Closed
        └── Filtered
```

---

# Running Nmap

Nmap is executed from the terminal.

Basic syntax:

```bash
nmap [options] <target>
```

Example:

```bash
nmap 10.10.10.10
```

---

# Nmap Help Menu

To display the help menu:

```bash
nmap -h
```

---

# Nmap Manual

To open the manual page:

```bash
man nmap
```

The manual provides detailed information about available options and switches.

---

# Scan Types Covered in this Room

The room introduces the following scan types:

### TCP Scans

- TCP Connect Scan (-sT)
- SYN Scan (-sS)

### UDP Scan

- UDP Scan (-sU)

### Stealth TCP Scans

- NULL Scan (-sN)
- FIN Scan (-sF)
- Xmas Scan (-sX)

### Network Discovery

- Ping Sweep (-sn)

---

# Key Points

- Enumeration is the first step before exploitation.
- Port scanning identifies available network services.
- Every computer has 65,535 ports.
- Open ports indicate running services.
- Closed ports have no active service.
- Filtered ports are usually protected by a firewall.
- Nmap is the industry standard tool for port scanning.
- Nmap is executed from the terminal.

---


---

# TCP Connect Scan (-sT)

## What is a TCP Connect Scan?

A TCP Connect Scan is one of the basic scan types used by Nmap.

It works by completing the **TCP Three-Way Handshake** with each target port.

If the connection is successful, Nmap marks the port as **Open**.

If the connection fails, Nmap determines whether the port is **Closed** or **Filtered**.

---

# TCP Three-Way Handshake

Before two devices communicate using TCP, they establish a connection through a process called the **Three-Way Handshake**.

The handshake consists of three steps.

---

## Step 1 – SYN

The client sends a packet with the **SYN** flag set.

This is a request to start a connection.

```text
Client
   │
   │ SYN
   ▼
Server
```

---

## Step 2 – SYN/ACK

If the port is open, the server responds with a packet containing both the **SYN** and **ACK** flags.

This tells the client that the server is ready to establish the connection.

```text
Client
   │
   │ SYN
   ▼
Server
   ▲
   │ SYN/ACK
   │
Client
```

---

## Step 3 – ACK

The client sends an **ACK** packet.

This completes the connection.

```text
Client
   │ SYN
   ▼
Server
   ▲ SYN/ACK
   │
Client
   │ ACK
   ▼
Server
```

The TCP connection is now fully established.

---

# How TCP Connect Scan Works

Nmap performs the complete TCP Three-Way Handshake on every target port.

For each port, it:

1. Sends a SYN packet.
2. Waits for the response.
3. Determines the port state.
4. Completes the handshake if the port is open.

---

# Open Port

If the target port is open:

1. Nmap sends a SYN packet.
2. The server replies with SYN/ACK.
3. Nmap sends ACK.
4. The port is marked **Open**.

Flow:

```text
Nmap
 │
 │ SYN
 ▼
Target
 ▲
 │ SYN/ACK
 │
Nmap
 │
 │ ACK
 ▼
Target

Result → OPEN
```

---

# Closed Port

If the port is closed:

1. Nmap sends a SYN packet.
2. The server immediately responds with an **RST (Reset)** packet.

The handshake is never completed.

Flow:

```text
Nmap
 │
 │ SYN
 ▼
Target
 ▲
 │ RST
 │
Nmap

Result → CLOSED
```

---

# Filtered Port

Sometimes a firewall blocks the request.

In this case:

1. Nmap sends a SYN packet.
2. No response is received.

Nmap marks the port as **Filtered**.

Flow:

```text
Nmap
 │
 │ SYN
 ▼
Firewall
 │
 │ Packet Dropped
 ▼

Result → FILTERED
```

---

# Firewall Behaviour

The room explains that many firewalls simply **drop incoming packets**.

When this happens:

- Nmap receives no reply.
- The port is identified as **Filtered**.

Some firewalls may instead send an **RST** packet, making the port appear closed.

This can make accurate detection more difficult.

---

# Command

Syntax:

```bash
nmap -sT <TARGET_IP>
```

Example:

```bash
nmap -sT 10.10.10.10
```

---

# Command Breakdown

| Option | Purpose |
|---------|---------|
| nmap | Starts Nmap |
| -sT | Performs a TCP Connect Scan |
| TARGET_IP | Target machine IP address |

---

# TCP Connect Scan Flow

```text
Start Scan
      │
      ▼
Send SYN
      │
      ▼
Receive Response
      │
      ├── SYN/ACK → Send ACK → OPEN
      │
      ├── RST → CLOSED
      │
      └── No Response → FILTERED
```

---

# Key Points

- TCP Connect Scan uses the complete TCP Three-Way Handshake.
- Open ports respond with **SYN/ACK**.
- Closed ports respond with **RST**.
- Filtered ports usually give no response.
- Firewalls can affect scan results.
- TCP Connect Scan is one of the basic scan types in Nmap.

---


---

# SYN Scan (-sS)

## What is a SYN Scan?

A SYN Scan is a TCP scan used to identify open ports without completing the full TCP Three-Way Handshake.

It is also known as:

- Half-Open Scan
- Stealth Scan

Unlike a TCP Connect Scan, a SYN Scan stops before the connection is fully established.

---

# How SYN Scan Works

A SYN Scan follows a different process when scanning an open port.

Instead of completing the handshake, Nmap resets the connection after receiving a response.

---

## Step 1 – Send SYN

Nmap sends a TCP packet with the **SYN** flag set.

This requests a connection with the target port.

```text
Nmap
  │
  │ SYN
  ▼
Target
```

---

## Step 2 – Receive SYN/ACK

If the port is open, the target responds with a **SYN/ACK** packet.

This indicates that the service is accepting connections.

```text
Nmap
  │ SYN
  ▼
Target
  ▲
  │ SYN/ACK
  │
Nmap
```

---

## Step 3 – Send RST

Instead of sending an ACK to complete the connection, Nmap immediately sends an **RST (Reset)** packet.

This closes the connection before it is fully established.

```text
Nmap
  │ SYN
  ▼
Target
  ▲ SYN/ACK
  │
Nmap
  │ RST
  ▼
Target
```

The port is marked as **Open**.

---

# Open Port

If the port is open:

- SYN is sent.
- SYN/ACK is received.
- RST is sent.
- Port is marked **Open**.

---

# Closed Port

If the port is closed:

- SYN is sent.
- The server replies with **RST**.

The port is marked **Closed**.

Flow:

```text
Nmap
 │
 │ SYN
 ▼
Target
 ▲
 │ RST
 │
Nmap

Result → CLOSED
```

---

# Filtered Port

If a firewall blocks the request:

- SYN is sent.
- No response is received.

The port is marked **Filtered**.

Flow:

```text
Nmap
 │
 │ SYN
 ▼
Firewall
 │
 │ Packet Dropped
 ▼

Result → FILTERED
```

---

# Why is it called a Half-Open Scan?

The TCP connection is never fully established.

The scan ends after receiving the SYN/ACK response by sending an RST packet.

Only part of the TCP Three-Way Handshake is completed.

---

# Advantages of SYN Scan

According to the room:

- Can bypass some older Intrusion Detection Systems (IDS).
- Applications often do not log the connection because it is never fully established.
- Faster than a TCP Connect Scan because the full handshake is not completed.

---

# Disadvantages of SYN Scan

According to the room:

- Requires **sudo** privileges on Linux.
- Uses raw packets, which require root permissions.
- Can sometimes cause unstable services to stop responding.

---

# Default Behavior

If Nmap is run with **sudo**, SYN Scan becomes the default TCP scan.

If Nmap is run **without sudo**, it automatically performs a **TCP Connect Scan (-sT)** instead.

---

# Command

Syntax:

```bash
sudo nmap -sS <TARGET_IP>
```

Example:

```bash
sudo nmap -sS 10.10.10.10
```

---

# Command Breakdown

| Option | Purpose |
|---------|----------|
| sudo | Runs with root privileges |
| nmap | Starts Nmap |
| -sS | Performs a SYN Scan |
| TARGET_IP | Target machine |

---

# SYN Scan Flow

```text
Start Scan
      │
      ▼
Send SYN
      │
      ▼
Receive Response
      │
      ├── SYN/ACK → Send RST → OPEN
      │
      ├── RST → CLOSED
      │
      └── No Response → FILTERED
```

---

# Key Points

- SYN Scan is also called a Half-Open Scan.
- It does not complete the TCP Three-Way Handshake.
- Open ports respond with SYN/ACK.
- Nmap immediately sends RST.
- Closed ports return RST.
- Filtered ports usually give no response.
- Requires root privileges on Linux.
- Faster than TCP Connect Scan.

---


---

# UDP Scan (-sU)

## What is a UDP Scan?

A UDP Scan is used to identify open **UDP ports** on a target machine.

Unlike TCP, UDP does **not** establish a connection before sending data.

UDP is a **stateless protocol**, meaning there is no TCP Three-Way Handshake.

Because there is no handshake, UDP scanning is slower and more difficult than TCP scanning.

---

# How UDP Works

UDP communication is very simple.

The client sends a UDP packet to the target port.

Unlike TCP, the target is **not required** to acknowledge the packet.

The sender simply sends the packet and waits to see if any response is received.

---

# UDP Scan Process

When Nmap performs a UDP scan, it sends a UDP packet to the target port.

The response determines the state of the port.

There are three possible results.

---

# Open or Filtered Port

If the target port is open, there is usually **no response**.

If the packet is blocked by a firewall, there is also **no response**.

Because both situations behave the same way, Nmap cannot distinguish between them.

The result is shown as:

```text
open|filtered
```

Flow:

```text
Nmap
 │
 │ UDP Packet
 ▼
Target

(No Response)

Result → OPEN | FILTERED
```

---

# Open Port

Sometimes an application replies to the UDP request.

When this happens, Nmap confirms that the port is open.

This response is uncommon.

Flow:

```text
Nmap
 │
 │ UDP Packet
 ▼
Target
 ▲
 │ UDP Response
 │
Nmap

Result → OPEN
```

---

# Closed Port

If the UDP port is closed, the target responds with an **ICMP Port Unreachable** message.

Nmap marks the port as **Closed**.

Flow:

```text
Nmap
 │
 │ UDP Packet
 ▼
Target
 ▲
 │ ICMP Port Unreachable
 │
Nmap

Result → CLOSED
```

---

# Why UDP Scans Are Slow

The room explains that UDP scanning is much slower than TCP scanning.

Reasons include:

- UDP has no handshake.
- No response usually means either open or filtered.
- Nmap sends another request to verify the result.

Only after receiving no response twice does Nmap report:

```text
open|filtered
```

---

# Using Top Ports

Scanning every UDP port takes a long time.

The room recommends scanning only the most common UDP ports.

Command:

```bash
nmap -sU --top-ports 20 <TARGET_IP>
```

This scans only the 20 most commonly used UDP ports.

---

# Protocol-Specific Payloads

Normally, Nmap sends empty UDP packets.

For well-known services, Nmap sends protocol-specific payloads.

These payloads increase the chance of receiving a response from the service.

This helps improve scan accuracy.

---

# Command

Basic Syntax:

```bash
sudo nmap -sU <TARGET_IP>
```

Example:

```bash
sudo nmap -sU 10.10.10.10
```

---

# Command Breakdown

| Option | Purpose |
|---------|----------|
| sudo | Runs with root privileges |
| nmap | Starts Nmap |
| -sU | Performs a UDP Scan |
| TARGET_IP | Target machine |

---

# Recommended Command

```bash
sudo nmap -sU --top-ports 20 <TARGET_IP>
```

### Breakdown

| Option | Purpose |
|---------|----------|
| --top-ports 20 | Scans only the top 20 most common UDP ports |

---

# UDP Scan Flow

```text
Start Scan
      │
      ▼
Send UDP Packet
      │
      ▼
Receive Response
      │
      ├── UDP Response → OPEN
      │
      ├── ICMP Port Unreachable → CLOSED
      │
      └── No Response → OPEN | FILTERED
```

---

# Key Points

- UDP is a stateless protocol.
- UDP does not use the TCP Three-Way Handshake.
- Open UDP ports usually do not respond.
- Closed UDP ports return an ICMP Port Unreachable message.
- Nmap reports many UDP ports as **open|filtered**.
- UDP scans are slower than TCP scans.
- `--top-ports` reduces scan time.
- Nmap sends protocol-specific payloads for well-known UDP services.

---


---

# NULL Scan (-sN), FIN Scan (-sF) and Xmas Scan (-sX)

## Introduction

NULL, FIN, and Xmas scans are less commonly used than TCP Connect, SYN, or UDP scans.

These scans are mainly used because they can be **stealthier** than a SYN Scan.

All three scans work in a similar way.

Instead of sending a normal TCP packet, they send **malformed TCP packets** and observe how the target responds.

---

# NULL Scan (-sN)

## What is a NULL Scan?

A NULL Scan sends a TCP packet with **no flags set**.

The packet is completely empty.

---

## How NULL Scan Works

### Step 1

Nmap sends a TCP packet with **no TCP flags**.

```text
Nmap
   │
   │ NULL Packet
   ▼
Target
```

---

### Step 2

If the port is **closed**, the target replies with an **RST** packet.

```text
Nmap
   │ NULL
   ▼
Target
   ▲
   │ RST
   │
Nmap

Result → CLOSED
```

---

### Step 3

If the port is **open**, the target does **not respond**.

Nmap marks it as:

```text
Open | Filtered
```

---

# FIN Scan (-sF)

## What is a FIN Scan?

A FIN Scan sends a packet with only the **FIN flag** set.

Normally, the FIN flag is used to gracefully close an existing TCP connection.

---

## How FIN Scan Works

### Step 1

Nmap sends a packet with the FIN flag.

```text
Nmap
   │
   │ FIN
   ▼
Target
```

---

### Step 2

If the port is **closed**, the target replies with **RST**.

Result:

```text
CLOSED
```

---

### Step 3

If the port is **open**, there is **no response**.

Result:

```text
Open | Filtered
```

---

# Xmas Scan (-sX)

## What is a Xmas Scan?

A Xmas Scan sends a TCP packet with three flags enabled:

- FIN
- PSH
- URG

When viewed in Wireshark, these flags resemble a blinking Christmas tree.

---

## How Xmas Scan Works

### Step 1

Nmap sends a packet with:

- FIN
- PSH
- URG

```text
Nmap
   │
   │ FIN + PSH + URG
   ▼
Target
```

---

### Step 2

If the port is **closed**, the target returns **RST**.

Result:

```text
CLOSED
```

---

### Step 3

If the port is **open**, no response is received.

Result:

```text
Open | Filtered
```

---

# Open Port Behaviour

For all three scans:

- NULL
- FIN
- Xmas

An **open port normally does not respond**.

Because a firewall may also drop the packet, Nmap cannot distinguish between the two situations.

Therefore the result becomes:

```text
Open | Filtered
```

---

# Closed Port Behaviour

If the port is closed:

The target returns:

```text
RST
```

Nmap marks the port as:

```text
Closed
```

---

# Filtered Port Behaviour

If the target returns an **ICMP Unreachable** message, Nmap marks the port as:

```text
Filtered
```

---

# Windows Limitation

The room explains that some operating systems do not follow the expected RFC behaviour.

Examples include:

- Microsoft Windows
- Many Cisco devices

These systems often respond with **RST** to every malformed TCP packet.

As a result, every port may appear as:

```text
Closed
```

even if some ports are actually open.

---

# Firewall Evasion

Many firewalls block packets that contain the **SYN** flag.

Since NULL, FIN, and Xmas scans **do not use SYN**, they may bypass these firewalls.

However, the room notes that modern IDS and firewall solutions can often detect these scan types.

---

# Commands

### NULL Scan

```bash
sudo nmap -sN <TARGET_IP>
```

---

### FIN Scan

```bash
sudo nmap -sF <TARGET_IP>
```

---

### Xmas Scan

```bash
sudo nmap -sX <TARGET_IP>
```

---

# Command Breakdown

| Option | Purpose |
|----------|----------|
| -sN | NULL Scan |
| -sF | FIN Scan |
| -sX | Xmas Scan |

---

# Scan Flow

```text
Send Malformed Packet
          │
          ▼
Receive Response
          │
          ├── RST → CLOSED
          │
          ├── No Response → OPEN | FILTERED
          │
          └── ICMP Unreachable → FILTERED
```

---

# Key Points

- NULL Scan sends no TCP flags.
- FIN Scan sends only the FIN flag.
- Xmas Scan sends FIN, PSH, and URG flags.
- Closed ports return RST.
- Open ports usually do not respond.
- No response is reported as Open|Filtered.
- These scans may bypass some SYN-based firewalls.
- Modern IDS solutions can often detect these scans.

---


---

# Ping Sweep (-sn)

## Introduction

When performing a black-box security assessment, the first objective is to identify which hosts are active on the network.

Before scanning ports, it is useful to know which IP addresses are alive.

Nmap performs this using a **Ping Sweep**.

---

# What is a Ping Sweep?

A Ping Sweep is a network discovery technique used to identify active hosts.

Instead of scanning ports, Nmap sends requests to multiple IP addresses and waits for responses.

If a host replies, Nmap considers it alive.

---

# How Ping Sweep Works

A Ping Sweep follows a simple process.

---

## Step 1 – Specify the Network

The user specifies a network range.

Example:

```text
192.168.0.1-254
```

or

```text
192.168.0.0/24
```

---

## Step 2 – Send Requests

Nmap sends requests to every IP address in the specified range.

The room explains that the **-sn** option primarily uses **ICMP Echo Requests**.

---

## Step 3 – Wait for Responses

Each device may respond.

If a response is received, the host is considered alive.

If no response is received, the host is considered inactive.

---

# Ping Sweep Flow

```text
Choose Network
        │
        ▼
Send ICMP Echo Requests
        │
        ▼
Receive Responses
        │
        ├── Response → Host Alive
        │
        └── No Response → No Reply
```

---

# The -sn Switch

The room uses the following option:

```bash
-sn
```

Purpose:

- Performs host discovery.
- Does **not** perform a port scan.

Instead of scanning services, Nmap focuses only on discovering active systems.

---

# Command Examples

## Using an IP Range

```bash
nmap -sn 192.168.0.1-254
```

This scans every address between:

```text
192.168.0.1
↓

192.168.0.254
```

---

## Using CIDR Notation

```bash
nmap -sn 192.168.0.0/24
```

This performs the same scan using CIDR notation.

---

# Additional Requests

The room explains that **-sn** does more than just send ICMP Echo Requests.

It also sends:

- TCP SYN packet to **Port 443**
- TCP ACK packet to **Port 80**

If Nmap is **not** running as root, it sends a **TCP SYN** packet instead of the ACK packet to Port 80.

---

# ARP Requests

When scanning a **local network**, the room states that Nmap may use **ARP Requests** instead of ICMP Echo Requests.

This occurs when:

- Running with **sudo**
- Running directly as the **root user**

---

# Command Breakdown

### Example

```bash
nmap -sn 192.168.0.0/24
```

| Option | Purpose |
|----------|----------|
| nmap | Starts Nmap |
| -sn | Performs host discovery only |
| 192.168.0.0/24 | Target network |

---

# Key Points

- Ping Sweep discovers active hosts.
- It does not scan ports.
- The **-sn** option enables host discovery.
- ICMP Echo Requests are primarily used.
- TCP SYN and TCP ACK packets may also be sent.
- ARP Requests may be used on local networks.
- Networks can be specified using IP ranges or CIDR notation.

---


---

# Nmap Scripting Engine (NSE)

## Introduction

The **Nmap Scripting Engine (NSE)** is a feature that extends the functionality of Nmap.

NSE scripts are written in the **Lua** programming language.

These scripts allow Nmap to perform more than just port scanning.

The room explains that NSE can be used for:

- Reconnaissance
- Vulnerability Scanning
- Authentication Checks
- Brute Force Attacks
- Automated Exploitation

---

# What is NSE?

NSE stands for **Nmap Scripting Engine**.

It is a collection of scripts that run against discovered services.

Instead of manually performing many tasks, NSE scripts can automate them.

---

# Why Use NSE?

After identifying open ports and services, NSE scripts help collect additional information or perform specific tasks against those services.

The room highlights that the NSE library contains many scripts for different purposes.

---

# NSE Categories

The room introduces several script categories.

---

## Safe

Purpose:

Scripts that **do not affect** the target system.

Example:

```bash
--script=safe
```

---

## Intrusive

Purpose:

Scripts that **may affect** the target.

Example:

```bash
--script=intrusive
```

---

## Vulnerability (vuln)

Purpose:

Scans the target for known vulnerabilities.

Example:

```bash
--script=vuln
```

---

## Exploit

Purpose:

Attempts to exploit a discovered vulnerability.

Example:

```bash
--script=exploit
```

---

## Authentication (auth)

Purpose:

Attempts to bypass or test authentication mechanisms.

Example:

Anonymous FTP login checks.

Example:

```bash
--script=auth
```

---

## Brute

Purpose:

Attempts to brute-force credentials.

Example:

```bash
--script=brute
```

---

## Discovery

Purpose:

Collects additional information from running services.

Example:

Queries services for more information.

```bash
--script=discovery
```

---

# Running a Script Category

Syntax:

```bash
nmap --script=<category> <TARGET_IP>
```

Example:

```bash
nmap --script=vuln 10.10.10.10
```

This runs all applicable vulnerability scripts against the target.

---

# NSE Workflow

```text
Port Scan
      │
      ▼
Identify Running Services
      │
      ▼
Select Script Category
      │
      ▼
Run NSE Scripts
      │
      ▼
Collect Results
```

---

# Command Breakdown

Example:

```bash
nmap --script=vuln <TARGET_IP>
```

| Option | Purpose |
|----------|----------|
| nmap | Starts Nmap |
| --script | Runs NSE scripts |
| vuln | Vulnerability category |
| TARGET_IP | Target machine |

---

# Key Points

- NSE extends Nmap's functionality.
- Scripts are written in Lua.
- Scripts only run against matching services.
- Different categories perform different tasks.
- NSE can automate reconnaissance and vulnerability detection.

---


---

# Running NSE Scripts

## Introduction

After learning about NSE categories, the next step is understanding how to run individual scripts.

The room explains that Nmap allows users to run:

- A complete script category.
- A specific script.
- Multiple scripts.
- Scripts with arguments.

---

# Running a Script Category

A complete category of scripts can be executed using the **--script** option.

Syntax:

```bash
nmap --script=<category> <TARGET_IP>
```

Example:

```bash
nmap --script=safe 10.10.10.10
```

Only scripts that match active services on the target will run.

---

# Running a Specific Script

Instead of an entire category, a single NSE script can be executed.

Syntax:

```bash
nmap --script=<script-name> <TARGET_IP>
```

Example:

```bash
nmap --script=http-fileupload-exploiter 10.10.10.10
```

This runs only the specified script.

---

# Running Multiple Scripts

More than one script can be executed in the same scan.

Multiple script names are separated by commas.

Syntax:

```bash
nmap --script=<script1>,<script2> <TARGET_IP>
```

Example:

```bash
nmap --script=smb-enum-users,smb-enum-shares 10.10.10.10
```

Both scripts are executed during the same scan.

---

# Using Script Arguments

Some NSE scripts require additional information before they can run.

These values are supplied using the **--script-args** option.

Syntax:

```bash
nmap --script <script-name> --script-args <arguments>
```

---

# Example

The room provides the following example:

```bash
nmap -p 80 --script http-put --script-args http-put.url='/dav/shell.php',http-put.file='./shell.php'
```

---

# Command Breakdown

### nmap

Starts Nmap.

---

### -p 80

Scans Port **80**.

---

### --script http-put

Runs the **http-put** NSE script.

---

### --script-args

Supplies arguments required by the script.

---

### http-put.url

Specifies the upload destination.

Example:

```text
/dav/shell.php
```

---

### http-put.file

Specifies the local file to upload.

Example:

```text
./shell.php
```

---

# Viewing Script Help

Each NSE script includes its own help menu.

Syntax:

```bash
nmap --script-help <script-name>
```

Example:

```bash
nmap --script-help http-put
```

This displays information about the selected script and its available arguments.

---

# Running Script Flow

```text
Start Nmap
      │
      ▼
Select Script
      │
      ▼
(Optional)
Add Script Arguments
      │
      ▼
Run Script
      │
      ▼
Collect Results
```

---

# Key Points

- `--script` runs NSE scripts.
- A single script can be executed by specifying its name.
- Multiple scripts are separated using commas.
- `--script-args` supplies required script arguments.
- `--script-help` displays information about an NSE script.

---



---

# Searching and Installing NSE Scripts

## Introduction

Nmap includes a large collection of NSE scripts.

Before running a script, you may need to find its name or check whether it is already installed.

The room explains several ways to search for NSE scripts and how to install new ones if required.

---

# Where are NSE Scripts Stored?

On Linux systems, Nmap stores its scripts in the following directory:

```text
/usr/share/nmap/scripts
```

When you specify a script using the **--script** option, Nmap searches this directory.

---

# Searching Scripts Using script.db

Inside the scripts directory, there is a file called:

```text
script.db
```

This file contains information about all available NSE scripts and their categories.

It can be searched using **grep**.

---

# Searching with grep

Syntax:

```bash
grep "<keyword>" /usr/share/nmap/scripts/script.db
```

Example:

```bash
grep "ftp" /usr/share/nmap/scripts/script.db
```

This displays all NSE scripts related to **FTP**.

---

# Command Breakdown

| Command | Purpose |
|----------|----------|
| grep | Searches for matching text |
| "ftp" | Search keyword |
| /usr/share/nmap/scripts/script.db | Script database file |

---

# Searching Scripts Using ls

Another method is using the **ls** command.

Syntax:

```bash
ls -l /usr/share/nmap/scripts/*keyword*
```

Example:

```bash
ls -l /usr/share/nmap/scripts/*ftp*
```

The asterisk (`*`) acts as a wildcard before and after the search term.

---

# Searching Script Categories

Categories can also be searched with **grep**.

Example:

```bash
grep "safe" /usr/share/nmap/scripts/script.db
```

This displays scripts that belong to the **safe** category.

---

# Installing a New NSE Script

If a required script is not available locally, the room explains that it can be downloaded manually.

Example:

```bash
sudo wget -O /usr/share/nmap/scripts/<script-name>.nse https://svn.nmap.org/nmap/scripts/<script-name>.nse
```

This downloads the script into the Nmap scripts directory.

---

# Updating the Script Database

After installing a new script, Nmap must update its script database.

Command:

```bash
nmap --script-updatedb
```

This refreshes **script.db** so Nmap can recognize the newly added script.

---

# Search and Installation Flow

```text
Need a Script
       │
       ▼
Search script.db
       │
       ▼
Search Scripts Directory
       │
       ▼
Script Found?
       │
   ┌───┴────┐
   │        │
 Yes       No
   │        │
   │        ▼
   │   Download Script
   │        │
   │        ▼
   └──► Update Database
            │
            ▼
      Script Ready
```

---

# Key Points

- NSE scripts are stored in `/usr/share/nmap/scripts`.
- `script.db` keeps track of installed scripts.
- `grep` can search scripts by keyword or category.
- `ls` can also locate scripts using wildcards.
- Missing scripts can be downloaded manually.
- `nmap --script-updatedb` updates the script database after installing new scripts.

---


---

# Firewall Evasion

## Introduction

Firewalls are used to filter network traffic and protect systems from unauthorized access.

During port scanning, a firewall may block packets sent by Nmap, making the target appear unreachable.

The room explains several Nmap options that can help when scanning systems protected by firewalls.

---

# ICMP Blocking

Many Windows systems block **ICMP Echo Requests** by default.

Since Nmap normally checks if a host is alive before scanning, blocking ICMP can make the host appear offline.

As a result, Nmap may skip scanning the target.

---

# The -Pn Option

## What is -Pn?

The **-Pn** option tells Nmap:

**Do not perform host discovery. Assume the target is alive and begin scanning immediately.**

This allows Nmap to scan hosts even if they do not respond to ICMP requests.

---

# How -Pn Works

## Step 1

Nmap skips the ping check.

↓

## Step 2

The target is treated as alive.

↓

## Step 3

Port scanning begins immediately.

---

# Command

```bash
nmap -Pn <TARGET_IP>
```

Example:

```bash
nmap -Pn 10.10.10.10
```

---

# Command Breakdown

| Option | Purpose |
|---------|----------|
| nmap | Starts Nmap |
| -Pn | Skip host discovery |
| TARGET_IP | Target machine |

---

# Firewall Evasion Options

The room introduces several additional options.

---

# Fragment Packets (-f)

Option:

```bash
-f
```

Purpose:

Split packets into smaller fragments.

Smaller packets may be less likely to be detected by some firewalls or IDS solutions.

Example:

```bash
nmap -f <TARGET_IP>
```

---

# Specify MTU (--mtu)

Option:

```bash
--mtu <number>
```

Purpose:

Specify the Maximum Transmission Unit (MTU) used for packets.

The value must be a multiple of **8**.

Example:

```bash
nmap --mtu 32 <TARGET_IP>
```

---

# Scan Delay (--scan-delay)

Option:

```bash
--scan-delay <time>ms
```

Purpose:

Adds a delay between packets sent during the scan.

The room explains this can help on unstable networks and may help avoid time-based firewall or IDS detection.

Example:

```bash
nmap --scan-delay 100ms <TARGET_IP>
```

---

# Invalid Checksum (--badsum)

Option:

```bash
--badsum
```

Purpose:

Generate packets with an invalid checksum.

A normal TCP/IP stack should discard these packets.

The room explains that some firewalls may still respond, helping identify their presence.

Example:

```bash
nmap --badsum <TARGET_IP>
```

---

# Firewall Evasion Flow

```text
Start Scan
      │
      ▼
Host Discovery
      │
      ├── Use -Pn → Skip Ping
      │
      ▼
Choose Evasion Option
      │
      ├── -f
      ├── --mtu
      ├── --scan-delay
      └── --badsum
      │
      ▼
Begin Scan
```

---

# Key Points

- Some firewalls block ICMP Echo Requests.
- **-Pn** skips host discovery and assumes the host is alive.
- **-f** fragments packets.
- **--mtu** specifies packet size.
- **--scan-delay** adds a delay between packets.
- **--badsum** sends packets with an invalid checksum.
- These options may help in firewall evasion depending on the environment.

---


# NEW CHAPTER 

# NMAP BASIC PORT SCANS 

# Nmap - Basic Port Scans

> **Day 07 – TryHackMe: Nmap Basic Port Scans**

---

# Port States

## Introduction

When Nmap scans a target, it checks the state of each port.

Many people think a port is only **Open** or **Closed**, but in real environments firewalls and filtering devices affect the results.

Because of this, Nmap uses **six different port states**.

---

# 1. Open

An **Open** port means a service is actively listening on that port.

Nmap successfully communicates with the service.

Example:

```text
80/tcp open http
```

---

# 2. Closed

A **Closed** port means no service is listening on that port.

The port is reachable, but nothing is accepting connections.

Example:

```text
22/tcp closed ssh
```

---

# 3. Filtered

A **Filtered** port means Nmap cannot determine whether the port is open or closed.

Usually this happens because a firewall blocks the packets.

The request may never reach the target, or the response may never return.

Example:

```text
Filtered
```

---

# 4. Unfiltered

An **Unfiltered** port means the port is reachable.

However, Nmap still cannot determine whether it is open or closed.

The room explains this state is mainly seen when using:

```bash
-sA
```

(ACK Scan)

---

# 5. Open | Filtered

This state means Nmap cannot decide whether the port is:

- Open

or

- Filtered

Both situations produce similar behaviour.

Example:

```text
open|filtered
```

---

# 6. Closed | Filtered

This state means Nmap cannot determine whether the port is:

- Closed

or

- Filtered

Example:

```text
closed|filtered
```

---

# Port State Summary

| State | Meaning |
|---------|----------|
| Open | Service is listening |
| Closed | No service is listening |
| Filtered | Firewall prevents detection |
| Unfiltered | Reachable but state unknown |
| Open\|Filtered | Could be Open or Filtered |
| Closed\|Filtered | Could be Closed or Filtered |

---

# Port State Flow

```text
Start Scan
      │
      ▼
Receive Response
      │
      ├── Service Reply → OPEN
      │
      ├── No Service → CLOSED
      │
      ├── Firewall Blocks → FILTERED
      │
      ├── ACK Scan Result → UNFILTERED
      │
      ├── Unknown → OPEN|FILTERED
      │
      └── Unknown → CLOSED|FILTERED
```

---

# Key Points

- Nmap uses six different port states.
- Firewalls affect scan results.
- Open means a service is listening.
- Closed means the port is reachable but unused.
- Filtered usually indicates firewall filtering.
- Unfiltered is commonly seen with ACK scans.
- Open|Filtered and Closed|Filtered indicate uncertainty.

---


---

# TCP Header and TCP Flags

## Introduction

Every TCP packet contains a **TCP Header**.

The TCP header carries important information that helps devices establish, manage, and terminate TCP connections.

The room explains that the TCP header is **24 bytes** long.

Some important fields include:

- Source Port
- Destination Port
- Sequence Number
- Acknowledgement Number
- TCP Flags

For Nmap scanning, the most important part of the TCP header is the **TCP Flags**.

---

# TCP Header Overview

The TCP header contains:

- Source TCP Port
- Destination TCP Port
- Sequence Number
- Acknowledgement Number
- TCP Flags

Nmap changes different TCP flags depending on the scan type being used.

---

# TCP Flags

The room introduces six important TCP flags.

---

## URG (Urgent)

The **URG** flag indicates that the data being sent is urgent.

When this flag is set, the receiver processes the urgent data immediately instead of waiting for previous TCP segments.

---

## ACK (Acknowledgement)

The **ACK** flag confirms that a TCP segment has been successfully received.

It is used during normal TCP communication after a connection has been established.

---

## PSH (Push)

The **PSH** flag tells TCP to immediately pass the received data to the application.

Instead of waiting for additional packets, the data is delivered immediately.

---

## RST (Reset)

The **RST** flag resets an existing TCP connection.

It is also sent when a device receives traffic for a port where no service is listening.

Firewalls may also send RST packets.

---

## SYN (Synchronize)

The **SYN** flag is used to begin the TCP Three-Way Handshake.

It synchronizes sequence numbers between two devices before communication begins.

---

## FIN (Finish)

The **FIN** flag indicates that the sender has finished sending data.

It is used to gracefully close an existing TCP connection.

---

# TCP Flag Summary

| Flag | Purpose |
|-------|---------|
| URG | Indicates urgent data |
| ACK | Acknowledges received data |
| PSH | Immediately delivers data to the application |
| RST | Resets a TCP connection |
| SYN | Starts a TCP connection |
| FIN | Gracefully closes a TCP connection |

---

# TCP Flags in Nmap

Different Nmap scan types use different TCP flags.

| Scan | TCP Flag(s) Used |
|------|------------------|
| TCP Connect Scan | SYN → SYN/ACK → ACK |
| SYN Scan | SYN |
| NULL Scan | No Flags |
| FIN Scan | FIN |
| Xmas Scan | FIN + PSH + URG |

---

# TCP Header Flow

```text
TCP Packet
      │
      ▼
TCP Header
      │
      ├── Source Port
      ├── Destination Port
      ├── Sequence Number
      ├── Acknowledgement Number
      └── TCP Flags
             │
             ├── URG
             ├── ACK
             ├── PSH
             ├── RST
             ├── SYN
             └── FIN
```

---

# Key Points

- Every TCP packet contains a TCP header.
- The TCP header is 24 bytes long.
- Nmap relies on TCP flags during different scan types.
- SYN starts a connection.
- ACK acknowledges received data.
- FIN closes a connection.
- RST resets a connection.
- URG indicates urgent data.
- PSH immediately delivers data to the application.

---


---

# Practical Scan Commands

## Introduction

This room introduces several Nmap options that help control how ports are scanned.

Instead of scanning the default ports only, these options allow you to scan specific ports, all ports, or only the most common ports.

---

# TCP Connect Scan

## Command

```bash
nmap -sT MACHINE_IP
```

### Purpose

Performs a **TCP Connect Scan** by completing the TCP Three-Way Handshake.

The room explains that this is the default scan available for **unprivileged users**.

---

# Fast Scan

## Command

```bash
nmap -F MACHINE_IP
```

### Purpose

Scans the **100 most common ports** instead of the default 1000 ports.

This reduces the scan time.

---

# Sequential Port Scan

## Command

```bash
nmap -r MACHINE_IP
```

### Purpose

Scans ports in **consecutive order** instead of random order.

The room notes that this can be useful for checking whether ports consistently remain open.

---

# SYN Scan

## Command

```bash
sudo nmap -sS MACHINE_IP
```

### Purpose

Performs a **TCP SYN Scan**.

The room explains that this is the default scan type when running Nmap with **sudo** or as the **root** user.

---

# UDP Scan

## Command

```bash
sudo nmap -sU --top-ports 10 MACHINE_IP
```

### Purpose

Scans the **10 most common UDP ports**.

The room demonstrates this command to identify UDP services while reducing scan time.

---

# Command Summary

| Command | Purpose |
|----------|----------|
| `nmap -sT MACHINE_IP` | TCP Connect Scan |
| `nmap -F MACHINE_IP` | Scan top 100 common ports |
| `nmap -r MACHINE_IP` | Scan ports in sequential order |
| `sudo nmap -sS MACHINE_IP` | TCP SYN Scan |
| `sudo nmap -sU --top-ports 10 MACHINE_IP` | Scan top 10 UDP ports |

---

# Scan Workflow

```text
Choose Scan Type
        │
        ▼
TCP Connect (-sT)
        │
        ├── SYN Scan (-sS)
        │
        ├── UDP Scan (-sU)
        │
        ├── Fast Scan (-F)
        │
        └── Sequential Scan (-r)
```

---

# Key Points

- `-sT` performs a TCP Connect Scan.
- `-F` scans only the 100 most common ports.
- `-r` scans ports sequentially.
- `-sS` performs a SYN Scan and requires elevated privileges.
- `-sU --top-ports 10` scans only the top 10 UDP ports.

---


---

# Port Selection and Scan Performance

## Introduction

By default, Nmap scans the **1000 most common TCP ports**.

However, in real penetration tests, you may need to scan:

- A single port
- Multiple specific ports
- A range of ports
- All ports
- Only the most common ports

Nmap also provides options to control the **scan speed** and **performance**.

---

# Scanning Specific Ports

You can scan one or more specific ports using the **-p** option.

## Command

```bash
nmap -p22,80,443 MACHINE_IP
```

### Purpose

Scans only:

- Port 22
- Port 80
- Port 443

---

# Scanning a Port Range

Instead of listing every port, you can scan a range.

## Command

```bash
nmap -p1-1023 MACHINE_IP
```

### Purpose

Scans every port from:

```text
1 → 1023
```

---

Another example:

```bash
nmap -p20-25 MACHINE_IP
```

Scans:

```text
20
21
22
23
24
25
```

---

# Scanning All Ports

## Command

```bash
nmap -p- MACHINE_IP
```

### Purpose

Scans all **65535 ports**.

---

# Fast Scan

## Command

```bash
nmap -F MACHINE_IP
```

### Purpose

Scans only the **100 most common ports**.

Useful when you need faster results.

---

# Top Ports Scan

## Command

```bash
nmap --top-ports 10 MACHINE_IP
```

### Purpose

Scans only the **10 most common ports**.

Instead of scanning every port, Nmap checks only the most frequently used ports.

---

# Command Summary

| Command | Purpose |
|----------|----------|
| `-p22,80,443` | Scan specific ports |
| `-p1-1023` | Scan a port range |
| `-p20-25` | Scan a smaller range |
| `-p-` | Scan all 65535 ports |
| `-F` | Scan top 100 ports |
| `--top-ports 10` | Scan top 10 ports |

---

# Timing Templates

Nmap provides six timing templates.

These control how fast packets are sent during scanning.

---

## Timing Levels

| Option | Name |
|---------|------|
| `-T0` | paranoid |
| `-T1` | sneaky |
| `-T2` | polite |
| `-T3` | normal |
| `-T4` | aggressive |
| `-T5` | insane |

---

## Default Timing

If no timing option is specified,

Nmap uses:

```bash
-T3
```

(Normal)

---

## Slow Timing

Example:

```bash
nmap -T0 MACHINE_IP
```

Purpose:

The room explains that this timing sends packets very slowly.

Useful when stealth is more important.

---

## Aggressive Timing

Example:

```bash
nmap -T4 MACHINE_IP
```

Purpose:

Used for faster scanning.

The room notes this timing is commonly used during CTFs and practice environments.

---

## Fastest Timing

Example:

```bash
nmap -T5 MACHINE_IP
```

Purpose:

Performs scans at the highest speed.

The room explains that higher speed may reduce scan accuracy because packet loss becomes more likely.

---

# Packet Rate

Nmap allows you to control how many packets are sent every second.

---

## Minimum Rate

Command:

```bash
nmap --min-rate 100 MACHINE_IP
```

Purpose:

Sets the minimum packet sending rate.

---

## Maximum Rate

Command:

```bash
nmap --max-rate 10 MACHINE_IP
```

Purpose:

Limits the scanner so it does not send more than the specified number of packets per second.

---

# Parallelism

Nmap can send multiple probes simultaneously.

The room refers to this as **probing parallelisation**.

---

## Minimum Parallelism

Command:

```bash
nmap --min-parallelism 512 MACHINE_IP
```

Purpose:

Maintains at least **512 probes** running in parallel.

---

## Maximum Parallelism

Command:

```bash
nmap --max-parallelism 512 MACHINE_IP
```

Purpose:

Limits the maximum number of parallel probes.

---

# Scan Performance Flow

```text
Choose Ports
      │
      ▼
Choose Timing
      │
      ▼
Choose Packet Rate
      │
      ▼
Choose Parallelism
      │
      ▼
Start Scan
```

---

# Key Points

- `-p` scans specific ports.
- `-p-` scans all 65535 ports.
- `-F` scans the top 100 ports.
- `--top-ports` scans the most common ports.
- `-T0` to `-T5` control scan speed.
- `--min-rate` and `--max-rate` control packet transmission.
- `--min-parallelism` and `--max-parallelism` control the number of simultaneous probes.

---



# NEW CHAPTER 

> ** Nmap Live Host Discovery**

---

# Introduction

Before performing a port scan, we first need to know **which hosts are online**.

Scanning offline systems wastes:

- Time
- Network resources
- Scan packets

Host Discovery is the process of identifying **live hosts** before starting service enumeration or port scanning.

Nmap provides multiple host discovery techniques using different network protocols.

---

# Why Host Discovery?

Host Discovery helps answer the first question during reconnaissance:

```text
Which systems are online?
```

Only after identifying live systems should port scanning begin.

This makes scanning:

- Faster
- More efficient
- Less noisy

---

# Learning Objectives

After completing this room, you should understand:

- Why host discovery is important
- When to use ARP scanning
- When to use ICMP scanning
- When to use TCP ping scans
- When to use UDP ping scans
- How Nmap discovers live hosts before port scanning

---

# Network Segment vs Subnet

Before using Host Discovery, it is important to understand the difference between a **Network Segment** and a **Subnet**.

---

## Network Segment

A Network Segment is a group of devices connected through the same physical network.

Examples:

- Ethernet Switch
- Wi-Fi Access Point

The network segment refers to the **physical connection**.

---

## Subnet

A Subnet (Subnetwork) is a logical division of an IP network.

Each subnet has:

- Its own IP range
- Its own subnet mask
- A router connecting it to other networks

The subnet refers to the **logical connection**.

---

# Common Subnet Sizes

## /16

Subnet Mask

```text
255.255.0.0
```

Approximate Hosts

```text
65,000
```

---

## /24

Subnet Mask

```text
255.255.255.0
```

Approximate Hosts

```text
250
```

---

# Same Subnet vs Different Subnet

If your system is on the **same subnet** as the target:

Nmap can use:

- ARP

If your system is on a **different subnet**:

ARP cannot cross the router.

Instead, Nmap must use:

- ICMP
- TCP
- UDP

---

# Host Discovery Protocols

Nmap can use multiple protocols for discovering live hosts.

| Layer | Protocol |
|---------|----------|
| Link Layer | ARP |
| Network Layer | ICMP |
| Transport Layer | TCP |
| Transport Layer | UDP |

---

# Why Multiple Protocols?

Different environments block different packet types.

For example:

- Firewall blocks ICMP
- TCP still works

or

- TCP blocked
- UDP responds

Using multiple discovery techniques increases the chance of identifying live hosts.

---

# Host Discovery Workflow

```text
Choose Target
       │
       ▼
Choose Discovery Method
       │
       ├── ARP
       ├── ICMP
       ├── TCP
       └── UDP
       │
       ▼
Receive Response
       │
       ▼
Host is Online
       │
       ▼
Start Port Scan
```

---

# Key Points

- Host Discovery is performed before port scanning.
- It identifies online systems.
- ARP works only on the local subnet.
- ICMP, TCP and UDP can discover hosts across networks.
- Different discovery methods help bypass different network restrictions.

---


---

# Target Specification

## Introduction

Before Nmap can discover live hosts, it must know **which target(s)** to scan.

Nmap allows you to specify targets in different ways depending on the size of the network.

You can scan:

- A single target
- Multiple targets
- A range of IP addresses
- A subnet
- A file containing multiple targets

---

# Single Target

The simplest way is to specify one IP address.

## Command

```bash
nmap TARGET_IP
```

### Purpose

Scans one target system.

---

# Multiple Targets

You can scan multiple hosts in one command.

## Command

```bash
nmap TARGET_IP1 TARGET_IP2 TARGET_IP3
```

### Purpose

Scans multiple specified hosts.

---

# IP Address Range

Instead of writing every IP address manually, you can specify a range.

## Command

```bash
nmap 10.10.10.15-20
```

### Purpose

Scans:

```text
10.10.10.15
10.10.10.16
10.10.10.17
10.10.10.18
10.10.10.19
10.10.10.20
```

---

# Subnet Scan

You can scan an entire subnet using CIDR notation.

## Command

```bash
nmap TARGET_IP/24
```

Example:

```bash
nmap 10.10.10.0/24
```

### Purpose

Scans every valid host inside the subnet.

---

# Target List File

If you have many targets, you can store them inside a text file.

Example file:

```text
10.10.10.5
10.10.10.10
10.10.10.25
```

Run:

```bash
nmap -iL list_of_hosts.txt
```

---

## Command Breakdown

| Option | Purpose |
|---------|----------|
| -iL | Read targets from a file |
| list_of_hosts.txt | File containing target IP addresses |

---

# List Scan

Sometimes you only want to verify which hosts Nmap will process.

Without sending any scan packets.

The room introduces **List Scan**.

## Command

```bash
nmap -sL TARGET_IP/24
```

### Purpose

Displays the list of targets that Nmap would scan.

No host discovery or port scanning is performed.

---

# Reverse DNS Lookup

During a List Scan, Nmap attempts to resolve hostnames using Reverse DNS (rDNS).

This may reveal useful information about systems.

Example:

```text
mail.company.local
dc01.domain.local
webserver.local
```

---

# Disable DNS Resolution

If you do not want Nmap to perform DNS lookups, use:

```bash
nmap -n TARGET_IP/24
```

### Purpose

Disables DNS resolution.

Only IP addresses are displayed.

---

# Command Summary

| Command | Purpose |
|----------|----------|
| `nmap TARGET_IP` | Scan one host |
| `nmap TARGET_IP1 TARGET_IP2` | Scan multiple hosts |
| `nmap 10.10.10.15-20` | Scan an IP range |
| `nmap TARGET_IP/24` | Scan a subnet |
| `nmap -iL list_of_hosts.txt` | Read targets from a file |
| `nmap -sL TARGET_IP/24` | List targets only |
| `nmap -n TARGET_IP/24` | Disable DNS lookups |

---

# Target Selection Flow

```text
Choose Target
      │
      ├── Single Host
      ├── Multiple Hosts
      ├── IP Range
      ├── Subnet
      └── Target File
             │
             ▼
(Optional)
List Targets (-sL)
             │
             ▼
(Optional)
Disable DNS (-n)
             │
             ▼
Begin Host Discovery
```

---

# Key Points

- Nmap supports multiple ways to specify targets.
- IP ranges simplify scanning consecutive addresses.
- CIDR notation scans an entire subnet.
- `-iL` reads targets from a file.
- `-sL` lists targets without scanning.
- `-n` disables DNS lookups.

---


---

# Default Host Discovery Behaviour

## Introduction

When you run Nmap without specifying any host discovery options, Nmap automatically decides which discovery method to use.

The method depends on:

- Whether the target is on the same network or a different network.
- Whether the user has **root/sudo** privileges.

This allows Nmap to choose the most suitable discovery technique before starting a port scan.

---

# Why Host Discovery?

Before scanning ports, Nmap first checks whether the target is online.

If a host is offline:

- No port scan is performed.
- Time is saved.
- Unnecessary network traffic is avoided.

---

# Default Behaviour

## 1. Local Network (Privileged User)

If the target is on the **same subnet** and Nmap is run as **root** or with **sudo**, Nmap uses:

```text
ARP Requests
```

Reason:

ARP is the fastest and most reliable method for discovering live hosts on the local network.

---

## 2. Remote Network (Privileged User)

If the target is on a **different subnet**, Nmap uses multiple discovery methods together.

These include:

- ICMP Echo Request
- TCP ACK Ping (Port 80)
- TCP SYN Ping (Port 443)
- ICMP Timestamp Request

This increases the chance of detecting live systems even if one protocol is blocked.

---

## 3. Remote Network (Unprivileged User)

If Nmap is executed **without root or sudo privileges**, it cannot create raw packets.

Instead, it performs a normal TCP connection attempt by sending SYN packets to:

- Port 80
- Port 443

This allows basic host discovery without elevated permissions.

---

# Host Discovery Only

If you only want to discover live hosts and do **not** want to perform a port scan, use:

```bash
nmap -sn TARGET_IP/24
```

---

## Command Breakdown

| Option | Purpose |
|---------|----------|
| `nmap` | Start Nmap |
| `-sn` | Host discovery only (No port scan) |
| `TARGET_IP/24` | Target subnet |

---

# Default Discovery Flow

```text
Start Nmap
      │
      ▼
Is Target on Local Network?
      │
 ┌────┴────┐
 │         │
Yes        No
 │          │
 ▼          ▼
Run as   Run as
Root?    Root?
 │          │
 ├── Yes → ARP Scan
 │
 └── No
            │
            ├── Root
            │      │
            │      ├── ICMP Echo
            │      ├── TCP ACK (80)
            │      ├── TCP SYN (443)
            │      └── ICMP Timestamp
            │
            └── Normal User
                   │
                   └── TCP SYN (80 & 443)
```

---

# Default Probe Summary

| Environment | Discovery Method |
|-------------|------------------|
| Local + Root | ARP |
| Remote + Root | ICMP Echo + TCP ACK + TCP SYN + ICMP Timestamp |
| Remote + Normal User | TCP SYN (80 & 443) |

---

# Key Points

- Nmap automatically chooses a host discovery method.
- Local privileged scans use ARP.
- Remote privileged scans combine multiple protocols.
- Remote unprivileged scans rely on TCP connections.
- `-sn` performs host discovery without port scanning.

---


---

# ARP Scan (-PR)

## Introduction

ARP (Address Resolution Protocol) is a **Link Layer** protocol used to discover the **MAC Address** of a device using its IP address.

Nmap uses ARP during Host Discovery to identify **live hosts** on the **same subnet**.

Unlike ICMP or TCP, ARP works only on the local network because ARP packets **cannot pass through a router**.

---

# Why ARP Scan?

Before two devices communicate on an Ethernet or Wi-Fi network, they must know each other's **MAC Address**.

Nmap uses this process to determine whether a host is online.

If a host replies with its MAC address, the host is considered **alive**.

---

# When Can ARP Scan Be Used?

ARP Scan works only when:

- The scanner and target are on the **same subnet**
- The network uses Ethernet (802.3) or Wi-Fi (802.11)

If the target is on another subnet, ARP cannot reach it because routers do not forward ARP packets.

---

# How ARP Scan Works

### Step 1

Nmap sends an ARP Request to a target IP address.

```text
Who has 10.10.10.5?
Tell 10.10.10.100
```

---

### Step 2

If the target is online, it replies with its MAC address.

Example:

```text
10.10.10.5 is at
AA:BB:CC:DD:EE:FF
```

---

### Step 3

Nmap receives the ARP Reply.

---

### Step 4

The host is marked as:

```text
Host is Up
```

---

# ARP Scan Workflow

```text
Nmap
   │
   │ ARP Request
   ▼
Target Host
   │
   │ ARP Reply (MAC Address)
   ▼
Nmap
   │
   ▼
Host Detected as Alive
```

---

# ARP Scan Command

```bash
sudo nmap -PR -sn TARGET_IP/24
```

---

# Command Breakdown

| Option | Purpose |
|---------|----------|
| sudo | Run with elevated privileges |
| nmap | Start Nmap |
| -PR | Perform ARP Ping Scan |
| -sn | Host Discovery Only (No Port Scan) |
| TARGET_IP/24 | Target subnet |

---

# Practical Example

```bash
sudo nmap -PR -sn 10.10.10.0/24
```

### Purpose

- Send ARP Requests
- Discover live hosts
- Do not perform port scanning

---

# Wireshark Observation

During an ARP Scan, Wireshark will show:

1. ARP Request packets
2. Broadcast MAC Address
3. Target IP Address
4. ARP Reply from live hosts

The room explains that:

- Requests are sent sequentially to every IP address.
- Online hosts respond with their MAC Address.
- Offline hosts do not reply.

---

# Why ARP Scan is Reliable

ARP operates at **Layer 2 (Link Layer)**.

Because of this:

- It does not rely on ICMP.
- It is commonly not filtered by local firewalls.
- It provides reliable results for devices on the same subnet.

---

# Practical Use Case

The room explains that ARP Scanning is especially useful during:

- Internal Network Enumeration
- Post-Exploitation
- Red Team Operations

After gaining access to a machine inside a network, ARP Scan can quickly identify other live systems on the same local segment.

---

# Key Points

- ARP works only on the local subnet.
- ARP discovers MAC addresses.
- A replying host is considered online.
- Routers do not forward ARP packets.
- `-PR` enables ARP Scan.
- `-sn` performs only Host Discovery.

---


---

# ICMP Host Discovery

## Introduction

ICMP (Internet Control Message Protocol) can also be used to discover live hosts.

Unlike ARP, ICMP works across different networks because it operates at the **Network Layer**.

However, the room explains that many firewalls block ICMP packets, so multiple ICMP techniques exist.

Nmap supports three ICMP host discovery methods:

- ICMP Echo Request (`-PE`)
- ICMP Timestamp Request (`-PP`)
- ICMP Address Mask Request (`-PM`)

---

# ICMP Echo Scan (-PE)

## What is ICMP Echo?

ICMP Echo sends an **ICMP Type 8 (Echo Request)** packet.

If the target is online, it replies with an **ICMP Type 0 (Echo Reply)**.

Receiving the reply confirms that the host is alive.

---

## How ICMP Echo Scan Works

### Step 1

Nmap sends an ICMP Echo Request.

```text
Echo Request
(Type 8)
```

↓

### Step 2

The target receives the packet.

↓

### Step 3

If the host is online, it returns an ICMP Echo Reply.

```text
Echo Reply
(Type 0)
```

↓

### Step 4

Nmap marks the host as:

```text
Host is Up
```

---

## Command

```bash
sudo nmap -PE -sn TARGET_IP/24
```

---

## Command Breakdown

| Option | Purpose |
|---------|----------|
| sudo | Run with elevated privileges |
| -PE | ICMP Echo Ping |
| -sn | Host Discovery Only |
| TARGET_IP/24 | Target subnet |

---

## Wireshark Observation

During this scan, Wireshark shows:

- ICMP Echo Requests
- ICMP Echo Replies
- One request sent to every IP address in the subnet

---

# ICMP Timestamp Scan (-PP)

## What is ICMP Timestamp?

Instead of sending an Echo Request, Nmap sends an **ICMP Timestamp Request (Type 13)**.

If the host is online, it responds with an **ICMP Timestamp Reply (Type 14)**.

---

## How Timestamp Scan Works

### Step 1

Nmap sends:

```text
Timestamp Request
(Type 13)
```

↓

### Step 2

Online hosts reply with:

```text
Timestamp Reply
(Type 14)
```

↓

### Step 3

Nmap marks the host as online.

---

## Command

```bash
sudo nmap -PP -sn TARGET_IP/24
```

---

## Command Breakdown

| Option | Purpose |
|---------|----------|
| sudo | Run with elevated privileges |
| -PP | ICMP Timestamp Scan |
| -sn | Host Discovery Only |

---

## Wireshark Observation

Wireshark displays:

- Timestamp Requests
- Timestamp Replies
- Requests sent to every valid IP address

---

# ICMP Address Mask Scan (-PM)

## What is ICMP Address Mask?

This method sends an **ICMP Address Mask Request (Type 17)**.

If supported, the host replies with an **ICMP Address Mask Reply (Type 18)**.

---

## How Address Mask Scan Works

### Step 1

Nmap sends:

```text
Address Mask Request
(Type 17)
```

↓

### Step 2

If the host supports this request, it replies.

↓

### Step 3

Nmap marks the host as online.

---

## Command

```bash
sudo nmap -PM -sn TARGET_IP/24
```

---

## Command Breakdown

| Option | Purpose |
|---------|----------|
| sudo | Run with elevated privileges |
| -PM | ICMP Address Mask Scan |
| -sn | Host Discovery Only |

---

## Important Note

The room demonstrates that this scan **may return no live hosts**.

Reason:

The target system or a firewall may block ICMP Address Mask requests.

Therefore, if this method fails, another discovery technique should be used.

---

# ICMP Scan Comparison

| Scan Type | Request | Reply |
|------------|---------|-------|
| ICMP Echo | Type 8 | Type 0 |
| Timestamp | Type 13 | Type 14 |
| Address Mask | Type 17 | Type 18 |

---

# ICMP Discovery Workflow

```text
Choose ICMP Method
        │
        ├── Echo (-PE)
        ├── Timestamp (-PP)
        └── Address Mask (-PM)
              │
              ▼
Send ICMP Request
              │
              ▼
Receive Reply
              │
        ┌─────┴─────┐
        │           │
      Reply      No Reply
        │           │
        ▼           ▼
 Host is Up   Try Another Method
```

---

# Key Points

- ICMP works across different networks.
- `-PE` uses Echo Requests.
- `-PP` uses Timestamp Requests.
- `-PM` uses Address Mask Requests.
- Firewalls may block ICMP traffic.
- Different ICMP methods improve host discovery success.

---


---

# TCP and UDP Host Discovery

## Introduction

Besides ARP and ICMP, Nmap can also use **TCP** and **UDP** packets to discover whether a host is online.

This is useful because:

- Some firewalls block ICMP.
- A host may still respond to TCP or UDP packets.
- Any valid response indicates that the host is online.

The room covers three methods:

- TCP SYN Ping (`-PS`)
- TCP ACK Ping (`-PA`)
- UDP Ping (`-PU`)

---

# TCP SYN Ping (-PS)

## What is TCP SYN Ping?

TCP SYN Ping sends a TCP packet with the **SYN flag** set to a target port.

By default, Nmap sends the SYN packet to **TCP Port 80** if no port is specified.

If the host is online:

- Open Port → SYN/ACK
- Closed Port → RST

Either response tells Nmap that the host is alive.

---

## How TCP SYN Ping Works

### Step 1

Nmap sends a TCP SYN packet.

↓

### Step 2

The target receives the packet.

↓

### Step 3

If the host is online:

Open Port

```text
SYN → SYN/ACK
```

Closed Port

```text
SYN → RST
```

↓

### Step 4

Nmap marks the host as:

```text
Host is Up
```

---

## Command

```bash
sudo nmap -PS -sn TARGET_IP/24
```

### Scan Specific Ports

```bash
sudo nmap -PS22,80,443 -sn TARGET_IP
```

---

## Command Breakdown

| Option | Purpose |
|---------|----------|
| sudo | Run as root |
| -PS | TCP SYN Ping |
| -sn | Host Discovery Only |
| TARGET_IP | Target Host |

---

## Wireshark Observation

The room shows:

- SYN packets sent.
- Open ports reply with SYN/ACK.
- Closed ports reply with RST.
- Nmap only checks for a response to determine whether the host is online.

---

# TCP ACK Ping (-PA)

## What is TCP ACK Ping?

TCP ACK Ping sends a packet with the **ACK flag** set.

Since no connection exists, the target replies with an **RST packet**.

Receiving this RST confirms that the host is online.

Default port:

```text
80
```

---

## How TCP ACK Ping Works

### Step 1

Nmap sends an ACK packet.

↓

### Step 2

The target receives the packet.

↓

### Step 3

The target replies:

```text
RST
```

↓

### Step 4

Nmap marks the host as:

```text
Host is Up
```

---

## Command

```bash
sudo nmap -PA -sn TARGET_IP/24
```

### Scan Specific Ports

```bash
sudo nmap -PA22,80,443 -sn TARGET_IP
```

---

## Command Breakdown

| Option | Purpose |
|---------|----------|
| sudo | Run as root |
| -PA | TCP ACK Ping |
| -sn | Host Discovery Only |

---

## Wireshark Observation

The room demonstrates:

- ACK packets sent.
- Online systems respond with RST.
- Offline systems do not respond.

---

# UDP Ping (-PU)

## What is UDP Ping?

UDP does not establish a connection.

Instead:

- Open UDP Port → Usually no reply.
- Closed UDP Port → ICMP Port Unreachable.

Receiving the ICMP error tells Nmap that the host is online.

---

## How UDP Ping Works

### Step 1

Nmap sends a UDP packet.

↓

### Step 2

If the UDP port is closed:

```text
ICMP Port Unreachable
```

↓

### Step 3

Nmap marks the host as:

```text
Host is Up
```

---

## Command

```bash
sudo nmap -PU -sn TARGET_IP/24
```

### Scan Specific UDP Ports

```bash
sudo nmap -PU53,161,162 -sn TARGET_IP
```

---

## Command Breakdown

| Option | Purpose |
|---------|----------|
| sudo | Run as root |
| -PU | UDP Ping |
| -sn | Host Discovery Only |

---

## Wireshark Observation

The room shows:

- UDP packets sent.
- Closed UDP ports generate ICMP Destination Unreachable.
- Nmap uses this response to identify live hosts.

---

# Comparison

| Method | Default Behaviour |
|---------|-------------------|
| TCP SYN (-PS) | SYN → SYN/ACK or RST |
| TCP ACK (-PA) | ACK → RST |
| UDP (-PU) | UDP → ICMP Port Unreachable |

---

# Commands Summary

```bash
sudo nmap -PS -sn TARGET_IP/24
```

TCP SYN Ping

---

```bash
sudo nmap -PS22,80,443 -sn TARGET_IP
```

TCP SYN Ping on specific ports

---

```bash
sudo nmap -PA -sn TARGET_IP/24
```

TCP ACK Ping

---

```bash
sudo nmap -PA22,80,443 -sn TARGET_IP
```

TCP ACK Ping on specific ports

---

```bash
sudo nmap -PU -sn TARGET_IP/24
```

UDP Ping

---

```bash
sudo nmap -PU53,161,162 -sn TARGET_IP
```

UDP Ping on specific ports

---

# Key Points

- TCP SYN Ping uses SYN packets.
- TCP ACK Ping uses ACK packets.
- UDP Ping relies on ICMP Port Unreachable replies.
- Any valid response confirms that the target host is online.
- `-sn` performs only host discovery.

---


---

# Reverse DNS Lookup

## What is Reverse DNS?

Reverse DNS (rDNS) is the opposite of normal DNS.

Instead of converting:

```text
Domain Name → IP Address
```

Reverse DNS converts:

```text
IP Address → Hostname
```

This helps identify the role of a system during reconnaissance.

Example:

```text
mail.company.local
dc01.domain.local
webserver.company.local
```

The room notes that Reverse DNS information may not always be available or accurate because not every system has rDNS records configured.

---

# Enable Reverse DNS Lookup

## Command

```bash
nmap -R TARGET_IP
```

### Command Breakdown

| Option | Purpose |
|---------|----------|
| -R | Perform Reverse DNS lookup for all hosts |
| TARGET_IP | Target host or subnet |

---

# Disable DNS Lookup

Sometimes you only want IP addresses and do not want Nmap to perform DNS resolution.

## Command

```bash
nmap -n TARGET_IP
```

### Command Breakdown

| Option | Purpose |
|---------|----------|
| -n | Disable DNS lookup |
| TARGET_IP | Target host or subnet |

---

# Masscan

The room briefly introduces **Masscan** as another network scanning tool.

Masscan uses a similar approach to discover systems, but it generates packets at a much faster rate than Nmap.

Because of this, Masscan is designed for high-speed network scanning.

---

## Example Commands

Scan Port 443

```bash
masscan TARGET_IP/24 -p443
```

---

Scan Ports 80 and 443

```bash
masscan TARGET_IP/24 -p80,443
```

---

Scan Port Range

```bash
masscan TARGET_IP/24 -p22-25
```

---

Install Masscan

```bash
sudo apt install masscan
```

---

# Host Discovery Commands Cheat Sheet

## ARP Scan

```bash
sudo nmap -PR -sn TARGET_IP/24
```

---

## ICMP Echo Scan

```bash
sudo nmap -PE -sn TARGET_IP/24
```

---

## ICMP Timestamp Scan

```bash
sudo nmap -PP -sn TARGET_IP/24
```

---

## ICMP Address Mask Scan

```bash
sudo nmap -PM -sn TARGET_IP/24
```

---

## TCP SYN Ping

```bash
sudo nmap -PS22,80,443 -sn TARGET_IP
```

---

## TCP ACK Ping

```bash
sudo nmap -PA22,80,443 -sn TARGET_IP
```

---

## UDP Ping

```bash
sudo nmap -PU53,161,162 -sn TARGET_IP
```

---

## List Scan

```bash
nmap -sL TARGET_IP/24
```

---

## Read Targets from File

```bash
nmap -iL list_of_hosts.txt
```

---

## Disable DNS Lookup

```bash
nmap -n TARGET_IP
```

---

## Reverse DNS Lookup

```bash
nmap -R TARGET_IP
```

---

# Important Options Summary

| Option | Purpose |
|----------|----------|
| -sn | Host Discovery Only |
| -PR | ARP Scan |
| -PE | ICMP Echo Scan |
| -PP | ICMP Timestamp Scan |
| -PM | ICMP Address Mask Scan |
| -PS | TCP SYN Ping |
| -PA | TCP ACK Ping |
| -PU | UDP Ping |
| -R | Reverse DNS Lookup |
| -n | Disable DNS Lookup |
| -iL | Read Targets from File |
| -sL | List Targets Only |

---

# Complete Host Discovery Workflow

```text
Choose Target
      │
      ▼
Specify Target
      │
      ├── Single Host
      ├── Multiple Hosts
      ├── Range
      ├── Subnet
      └── Target File
             │
             ▼
Choose Discovery Method
      │
      ├── ARP (-PR)
      ├── ICMP Echo (-PE)
      ├── ICMP Timestamp (-PP)
      ├── ICMP Address Mask (-PM)
      ├── TCP SYN (-PS)
      ├── TCP ACK (-PA)
      └── UDP (-PU)
             │
             ▼
Receive Response
             │
      ┌──────┴──────┐
      │             │
    Response     No Response
      │
      ▼
Host is Up
      │
      ▼
Start Port Scanning
```

---

# Key Takeaways

- Host Discovery is the first step before port scanning.
- ARP works only on the local subnet.
- ICMP works across different networks but may be blocked by firewalls.
- TCP SYN, TCP ACK, and UDP Ping provide alternative discovery methods.
- `-sn` performs only host discovery.
- `-R` enables Reverse DNS lookups.
- `-n` disables DNS lookups.
- Masscan is introduced as a faster alternative for network scanning.

---


