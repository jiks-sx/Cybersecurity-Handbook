# 100-Day Pentesting Challenge
## Day 12: Intro to Defensive Security — Blue Team Context

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| Blue Team vs Red Team | Understand both offensive and defensive security |
| SOC & Incident Response | Learn how defenders detect and respond to attacks |
| SIEM & Log Analysis | Understand what evidence attackers leave behind |
| Threat Intelligence | Learn how defenders identify attacker behavior |
| Digital Forensics | Understand how attacks are investigated |

> Every action performed during a penetration test leaves evidence. Understanding how defenders monitor systems helps you better understand the detection process.

---

## THM Rooms to Complete (In Order)

| # | Room | Focus |
|---|---|---|
| 1 | Incident Response and Forensics | Incident response lifecycle, containment, eradication |
| 2 | Cyber Threat Intelligence | Threat intelligence, IOCs, TTPs |
| 3 | Intro to SIEM | Log sources, alerts, correlation |

> All rooms are available on TryHackMe.

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:15 | Study Red Team, Blue Team and SOC concepts |
| 0:15 – 0:40 | Complete THM Rooms 1 and 2 |
| 0:40 – 0:55 | Study Incident Response and MITRE ATT&CK |
| 0:55 – 1:20 | Complete THM Room 3 |
| 1:20 – 2:00 | Review SIEM, logs and defensive monitoring |

---

## Must-Know Concepts

### 1. Red Team vs Blue Team vs Purple Team

```
RED TEAM
• Offensive Security
• Pentesters
• Ethical Hackers
• Bug Bounty Hunters

Goal:
Find vulnerabilities before attackers do.

Activities:
• Reconnaissance
• Exploitation
• Privilege Escalation
• Lateral Movement
• Post Exploitation

--------------------------------------------------

BLUE TEAM
• Defensive Security
• SOC Analysts
• Incident Responders
• Threat Hunters
• DFIR Specialists

Goal:
Detect, respond to and recover from attacks.

Activities:
• SIEM Monitoring
• Log Analysis
• Incident Response
• Digital Forensics
• Threat Hunting

--------------------------------------------------

PURPLE TEAM

Red Team + Blue Team working together.

Goal:
Improve both offensive and defensive capabilities.
```

---

### 2. Security Operations Center (SOC)

A Security Operations Center (SOC) continuously monitors an organization's infrastructure to identify suspicious activity and respond to security incidents.

---

### SOC Analyst Roles

| Role | Responsibility |
|---|---|
| Tier 1 Analyst | Monitor alerts and perform initial triage |
| Tier 2 Analyst | Investigate confirmed incidents and contain threats |
| Tier 3 Analyst | Threat hunting, malware analysis and detection engineering |
| SOC Manager | Manage operations and coordinate the security team |

---

### Typical SOC Monitoring Sources

```text
Network Traffic

Firewall Logs

IDS / IPS Alerts

DNS Queries

Authentication Logs

Windows Event Logs

Linux Syslogs

Cloud Logs

Endpoint Detection & Response (EDR)

Email Security

Web Proxy Logs
```

---

### 3. Common Attack Indicators

Security teams look for patterns that indicate malicious activity.

---

#### Network Indicators

| Activity | Typical Indicator |
|---|---|
| Port Scanning | Large number of connection attempts to different ports |
| Nmap Scan | Multiple SYN packets across many ports |
| Reverse Shell | Outbound connection to an unfamiliar external IP |
| Data Exfiltration | Large outbound data transfer |
| DNS Tunneling | High volume of unusual DNS requests |
| ARP Spoofing | Conflicting ARP responses |
| Command & Control | Regular outbound beacon traffic |

---

#### Endpoint Indicators

| Activity | Typical Indicator |
|---|---|
| Reverse Shell | Suspicious shell process |
| Credential Dumping | Access to authentication processes |
| Privilege Escalation | New high-privilege process |
| Persistence | New scheduled task or startup entry |
| Lateral Movement | Connections to multiple internal systems |
| Keylogging | Continuous keyboard monitoring process |

---

### Why Detection Matters

Every action performed during a penetration test creates logs.

Examples include:

- Port scanning
- Failed login attempts
- Successful authentication
- New process creation
- Scheduled task creation
- Service installation
- File modifications
- Network connections

Understanding these artifacts helps explain how security teams identify malicious activity and respond to incidents.

---

### 4. Incident Response Lifecycle

The **NIST Incident Response Framework** is one of the most widely adopted approaches for handling security incidents.

---

#### Incident Response Process

```text
Preparation
      │
      ▼
Detection
      │
      ▼
Analysis
      │
      ▼
Containment
      │
      ▼
Eradication
      │
      ▼
Recovery
      │
      ▼
Lessons Learned
```

---

#### Phase Overview

| Phase | Purpose |
|---|---|
| Preparation | Develop policies, tools, playbooks and train the response team |
| Detection | Identify suspicious activity and generate alerts |
| Analysis | Validate the incident and determine its scope |
| Containment | Isolate affected systems to stop further impact |
| Eradication | Remove malware, attacker access and vulnerabilities |
| Recovery | Restore systems and monitor for recurrence |
| Lessons Learned | Document findings and improve future response |

---

### What Happens During a Pentest

A mature security team may observe the following sequence:

```text
Port Scan

↓

SIEM Alert

↓

SOC Investigation

↓

Source IP Identification

↓

Affected Host Isolation

↓

Malicious Activity Removed

↓

Detection Rules Updated
```

---

### 5. MITRE ATT&CK Framework

MITRE ATT&CK is a publicly available knowledge base that documents real-world adversary tactics and techniques.

It provides a common language for both offensive and defensive security teams.

Official website:

```text
https://attack.mitre.org
```

---

### ATT&CK Tactics

| Tactic | Purpose |
|---|---|
| Reconnaissance | Gather target information |
| Resource Development | Prepare attacker infrastructure |
| Initial Access | Gain initial entry |
| Execution | Execute malicious code |
| Persistence | Maintain long-term access |
| Privilege Escalation | Obtain higher privileges |
| Defense Evasion | Avoid security controls |
| Credential Access | Obtain credentials |
| Discovery | Gather internal information |
| Lateral Movement | Move to additional systems |
| Collection | Gather valuable data |
| Command and Control | Maintain communication with compromised systems |
| Exfiltration | Transfer data outside the environment |
| Impact | Disrupt, encrypt or destroy systems |

---

### Why ATT&CK Matters

ATT&CK helps security teams:

- Understand attacker behavior
- Build detection rules
- Improve incident response
- Standardize reporting
- Map defensive controls

---

### 6. Threat Intelligence

Threat Intelligence helps organizations understand current attacker behavior and emerging threats.

---

### Types of Threat Intelligence

| Type | Description |
|---|---|
| Strategic | High-level trends for leadership |
| Tactical | Adversary tactics, techniques and procedures |
| Operational | Information about active campaigns |
| Technical | Indicators of Compromise (IOCs) |

---

### Indicators of Compromise (IOCs)

Common IOCs include:

```text
IP Addresses

Domain Names

URLs

Email Addresses

File Hashes

Registry Keys

File Paths

Mutex Names

SSL Certificates
```

---

### Threat Intelligence Platforms

| Platform | Purpose |
|---|---|
| VirusTotal | File, URL and IP reputation |
| AbuseIPDB | Malicious IP reporting |
| AlienVault OTX | Community threat intelligence |
| Shodan | Internet-connected device intelligence |
| MalwareBazaar | Malware samples |
| URLhaus | Malicious URL database |
| MISP | Threat intelligence sharing platform |
| Recorded Future | Commercial threat intelligence |

---

### 7. Security Information and Event Management (SIEM)

A SIEM collects logs from multiple systems, normalizes them, correlates events and generates alerts.

---

### Common SIEM Platforms

| SIEM | Description |
|---|---|
| Splunk | Enterprise log analysis platform |
| Microsoft Sentinel | Cloud-native SIEM |
| IBM QRadar | Enterprise security monitoring |
| Elastic SIEM | ELK-based monitoring platform |
| Wazuh | Open-source SIEM and HIDS |
| Graylog | Centralized log management |

---

### Typical SIEM Workflow

```text
Firewall Logs
        │
Windows Event Logs
        │
Linux Logs
        │
DNS Logs
        │
Proxy Logs
        │
EDR Alerts
        │
        ▼
      SIEM
        │
        ▼
Correlation
        │
        ▼
Alert
        │
        ▼
SOC Investigation
```

---

### Common Log Sources

| Source | Example Events |
|---|---|
| Windows Event Logs | Login events, process creation |
| Linux Auth Logs | SSH logins, sudo usage |
| Firewall Logs | Allowed and blocked connections |
| DNS Logs | DNS queries |
| Proxy Logs | Web browsing activity |
| IDS/IPS | Network attack signatures |
| Endpoint Detection | Malware and suspicious processes |

---

### 8. Windows Event IDs

Security teams frequently monitor these event IDs.

| Event ID | Description |
|---|---|
| 4624 | Successful login |
| 4625 | Failed login |
| 4648 | Explicit credential usage |
| 4672 | Privileged login |
| 4688 | New process created |
| 4698 | Scheduled task created |
| 4720 | User account created |
| 4732 | User added to privileged group |
| 7045 | Service installed |
| 4663 | Sensitive file access |

---

### Why These Events Matter

Many offensive actions generate Windows security events.

Examples include:

- Successful authentication
- Failed authentication
- Process execution
- Privilege escalation
- Scheduled task creation
- Service installation
- User creation
- Sensitive file access

Security teams use these events to identify suspicious activity and investigate incidents.

---

### 9. Digital Forensics

Digital Forensics focuses on collecting, preserving, analyzing and reporting digital evidence after a security incident.

The objective is to determine:

- What happened
- When it happened
- How it happened
- Who was involved
- What systems were affected

---

### Common Digital Evidence

| Evidence | Description |
|---|---|
| Windows Event Logs | Authentication, process creation and system events |
| Linux Authentication Logs | SSH logins, sudo usage and authentication events |
| Browser History | Websites accessed by the user |
| File System Metadata | File creation, modification and access timestamps |
| Registry | Windows configuration and persistence information |
| Prefetch Files | Recently executed applications |
| Scheduled Tasks | Persistence mechanisms |
| Cron Jobs | Scheduled Linux tasks |
| USB History | Connected removable devices |
| Memory Dumps | Running processes and volatile data |

---

### Common Linux Forensic Locations

```text
/var/log/auth.log

/var/log/syslog

~/.bash_history

/etc/crontab

/var/spool/cron

/tmp
```

---

### Common Windows Forensic Locations

```text
C:\Windows\System32\winevt\

C:\Windows\Prefetch\

C:\Users\<User>\AppData\

Windows Registry

Pagefile.sys

Recent Files
```

---

### Typical Forensic Questions

- Which account logged in?
- Which processes were executed?
- Which files were modified?
- Was privilege escalation performed?
- Was data exfiltrated?
- Was persistence established?

---

### 10. Defensive Security Tools

Security teams use multiple technologies to detect, investigate and respond to attacks.

| Tool | Category | Purpose |
|---|---|---|
| Splunk | SIEM | Log collection and analysis |
| Microsoft Sentinel | SIEM | Cloud security monitoring |
| Elastic SIEM | SIEM | Log correlation |
| Wazuh | HIDS / SIEM | Endpoint monitoring |
| Zeek | Network Monitoring | Network traffic analysis |
| Snort | IDS | Network intrusion detection |
| Suricata | IDS / IPS | Intrusion detection and prevention |
| CrowdStrike Falcon | EDR | Endpoint detection and response |
| Microsoft Defender | EDR | Endpoint protection |
| Velociraptor | DFIR | Digital forensics and incident response |
| TheHive | Incident Response | Case management |
| MISP | Threat Intelligence | Threat intelligence sharing |
| OpenVAS | Vulnerability Scanner | Security assessments |
| Nessus | Vulnerability Scanner | Vulnerability detection |

---

### 11. Red Team Operational Security (OPSEC)

Operational Security (OPSEC) helps reduce unnecessary exposure during authorized security assessments.

Good operational practices include:

```text
Use encrypted communication channels.

Limit unnecessary network traffic.

Remove temporary files after testing.

Document actions performed.

Use legitimate administrative tools when appropriate.

Understand which activities generate security alerts.
```

Avoid unnecessary noise during assessments:

```text
Excessive scanning

Repeated failed authentication

Leaving temporary files

Running unnecessary services

Ignoring system logs

Generating avoidable alerts
```

---

## Quick Reference Cheatsheet

```text
SOC
Tier 1 → Monitor
Tier 2 → Investigate
Tier 3 → Threat Hunt

Incident Response
Preparation
Detection
Analysis
Containment
Eradication
Recovery
Lessons Learned

MITRE ATT&CK
Reconnaissance
Initial Access
Execution
Persistence
Privilege Escalation
Defense Evasion
Credential Access
Discovery
Lateral Movement
Collection
Command and Control
Exfiltration
Impact

Common SIEM
Splunk
Sentinel
Elastic
Wazuh
Graylog

Important Windows Events
4624  Successful Login
4625  Failed Login
4688  Process Creation
4698  Scheduled Task
4720  User Creation
7045  Service Installation

Common Linux Logs
/var/log/auth.log
/var/log/syslog
~/.bash_history

Threat Intelligence
VirusTotal
AbuseIPDB
AlienVault OTX
Shodan
MISP
MalwareBazaar
```

---

## Day 12 Submission Task

```text
Complete all TryHackMe rooms listed above.

Study the MITRE ATT&CK framework.

Choose:

• Three Privilege Escalation techniques
• Three Defense Evasion techniques

Create short notes explaining each technique.

Write a short summary describing:

• What a SOC analyst would observe during a port scan
• Which logs would be generated
• Which defensive tools could detect the activity

Push everything to:

100-days-pentest/
└── Day-12/

Share your progress on Discord or Telegram.
```

---

### Bonus Challenge

```text
Choose one ATT&CK technique.

Research:

• Description
• Detection methods
• Possible mitigations

Document your findings in Markdown.
```

---

> **End of Day 12**

**Next Up:** **Day 13 — Hack The Box Starting Point: Meow**

---
