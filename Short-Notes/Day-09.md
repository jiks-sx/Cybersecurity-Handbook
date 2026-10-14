# 100-Day Pentesting Challenge
## Day 9: Google Dorking — OSINT & Advanced Search

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| Google Dorking | Discover publicly indexed files, login pages, and exposed information |
| OSINT Fundamentals | Collect intelligence without interacting with the target |
| Shodan | Search internet-connected devices and services |
| Metadata Analysis | Extract hidden information from documents and images |
| OSINT Frameworks | Organize investigations using professional tools |

> OSINT (Open Source Intelligence) allows security professionals to collect valuable information using only publicly available sources.

---

## THM Rooms to Complete (In Order)

| # | Room | Link | Focus |
|---|---|---|---|
| 1 | Google Dorking | https://tryhackme.com/room/googledorking | Search operators and exposed information |
| 2 | OHSINT | https://tryhackme.com/room/ohsint | Practical OSINT investigation |
| 3 | Shodan.io | https://tryhackme.com/room/shodan | Shodan search and filters |
| 4 | Searchlight — OSINT | https://tryhackme.com/room/searchlightosint | Practical OSINT challenges |

> All rooms are available on TryHackMe.

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:15 | Study Google search operators |
| 0:15 – 0:40 | Complete THM Room 1 |
| 0:40 – 0:55 | Learn OSINT methodology and Shodan |
| 0:55 – 1:20 | Complete THM Rooms 2 & 3 |
| 1:20 – 1:40 | Study metadata analysis |
| 1:40 – 2:00 | Complete THM Room 4 and update GitHub |

---

## Must-Know Concepts

### 1. What is Google Dorking?

Google Dorking (Google Hacking) is the use of advanced Google search operators to locate information already indexed by Google.

Examples include:

- Login portals
- Open directories
- Configuration files
- Backup files
- Public documents
- Source code
- Other publicly exposed resources

> Google Dorking should only be used to discover publicly available information. Accessing systems without authorization is prohibited.

---

### 2. Google Search Operators

| Operator | Purpose | Example |
|---|---|---|
| `site:` | Search within one domain | `site:example.com` |
| `intitle:` | Match page title | `intitle:"login"` |
| `allintitle:` | All words in title | `allintitle:admin login` |
| `inurl:` | Match URL | `inurl:admin` |
| `allinurl:` | Multiple URL keywords | `allinurl:admin login` |
| `intext:` | Match page content | `intext:"password"` |
| `filetype:` | Search by file type | `filetype:pdf` |
| `ext:` | File extension | `ext:log` |
| `cache:` | Cached page | `cache:example.com` |
| `link:` | Pages linking to target | `link:example.com` |
| `related:` | Similar websites | `related:github.com` |
| `" "` | Exact phrase | `"admin panel"` |
| `-` | Exclude keyword | `admin -wordpress` |
| `*` | Wildcard | `"password*.txt"` |
| `OR` | Logical OR | `login OR admin` |
| `..` | Number range | `2020..2024` |

---

### 3. Common Google Dorks

#### Login Pages

```text
intitle:"login" inurl:admin

inurl:"/wp-admin/login.php"

inurl:"/phpmyadmin"

intitle:"Webmin"

inurl:"/admin/login"

intitle:"Cisco" "Please Login"
```

---

#### Open Directories

```text
intitle:"index of" "parent directory"

intitle:"index of" ".git"

intitle:"index of" ".env"

intitle:"index of" "backup"

intitle:"index of" "credentials"

intitle:"index of" "wp-content"
```

---

#### Configuration & Sensitive Files

```text
filetype:env "DB_PASSWORD"

filetype:sql "INSERT INTO"

filetype:log "username"

filetype:xml "password"

filetype:cfg "password"

ext:ini

ext:bak

filetype:txt "username:password"

filetype:yaml "password"

filetype:conf
```

---

#### Database Files

```text
filetype:sql

filetype:mdb

filetype:sqlite

intitle:"index of" "database.sql"
```

---

#### Internet-Connected Devices

```text
intitle:"webcamXP"

intitle:"Live View"

inurl:"view.shtml"

intitle:"surveillance"
```

---

#### Public Documents

```text
site:target.com filetype:xls

site:target.com filetype:xlsx

site:target.com filetype:csv

site:docs.google.com confidential
```

---

#### Public Repositories

```text
site:github.com "api_key"

site:github.com "secret"

site:github.com "password"

site:gitlab.com "password"

site:pastebin.com "password"
```

---

#### Technology Discovery

```text
inurl:"/.git/config"

intitle:"phpinfo()"

inurl:"/server-status"

inurl:"/actuator/env"

inurl:"jmx-console"
```

---

### 4. Target-Specific Google Dorking

```bash
# Discover indexed pages
site:target.com

# Discover subdomains
site:*.target.com

# Search for login pages
site:target.com inurl:login

site:target.com intitle:"login"

# Search for admin pages
site:target.com inurl:admin

site:target.com intitle:"admin"

# Search for exposed documents
site:target.com filetype:pdf

site:target.com filetype:xlsx

site:target.com filetype:sql

site:target.com ext:env

site:target.com ext:log

site:target.com ext:bak

# Search for application errors
site:target.com "SQL syntax"

site:target.com "stack trace"

site:target.com "Fatal error"

# Search for development systems
site:target.com inurl:test

site:target.com inurl:dev

site:target.com inurl:staging

site:target.com inurl:backup
```

---
### 5. OSINT Investigation Methodology

A structured OSINT workflow helps ensure that information is collected efficiently and documented properly.

```text
Target
   │
   ▼
Collect Basic Information
   │
   ▼
Search Engines
   │
   ▼
Social Media
   │
   ▼
Domain & DNS Enumeration
   │
   ▼
Public Documents
   │
   ▼
Metadata Analysis
   │
   ▼
Internet-Connected Assets
   │
   ▼
Create Investigation Report
```

---

### 6. Common OSINT Sources

| Source | Information Collected |
|---|---|
| Google | Indexed pages, files, exposed resources |
| Bing | Alternative search results |
| GitHub | Source code, secrets, repositories |
| GitLab | Public repositories |
| LinkedIn | Employees, technologies, company structure |
| Twitter/X | Announcements, usernames |
| Facebook | Public profiles |
| Reddit | Discussions and technical information |
| Whois | Domain ownership |
| DNS Records | Nameservers, MX records, subdomains |
| Shodan | Internet-facing services |
| Censys | Internet infrastructure |
| crt.sh | Certificate Transparency logs |
| Wayback Machine | Historical website snapshots |

---

### 7. Shodan

Shodan indexes devices connected directly to the Internet.

Unlike Google, Shodan searches:

- Servers
- Routers
- Firewalls
- Cameras
- Industrial systems
- Databases
- IoT devices

---

#### Common Shodan Filters

```text
apache

nginx

country:US

country:IN

port:22

port:80

port:443

hostname:example.com

os:Windows

os:Linux

product:Apache

product:OpenSSH

city:London

org:"Google"

net:192.168.0.0/16
```

---

#### Useful Shodan Searches

```text
apache country:IN

nginx port:443

product:OpenSSH

product:MySQL

product:"Microsoft IIS"

os:Linux

hostname:target.com

ssl:"target.com"

http.title:"Admin Login"

port:3389
```

---

### 8. theHarvester

theHarvester collects publicly available information from multiple sources.

It can discover:

- Email addresses
- Subdomains
- Hosts
- IP addresses
- Employee names

---

#### Basic Usage

```bash
theHarvester -d target.com -b google

theHarvester -d target.com -b bing

theHarvester -d target.com -b yahoo

theHarvester -d target.com -b linkedin

theHarvester -d target.com -b github

theHarvester -d target.com -b all
```

---

#### Save Results

```bash
theHarvester -d target.com -b all -f report

theHarvester -d target.com -b google -f google_results
```

---

### 9. Whois Enumeration

Whois provides registration information for domains.

```bash
whois target.com

whois google.com
```

Common information:

- Registrar
- Creation date
- Expiration date
- Name servers
- Organization
- Contact information (when public)

---

### 10. DNS Enumeration

```bash
dig target.com

dig target.com MX

dig target.com NS

dig target.com TXT

host target.com

nslookup target.com
```

Useful information includes:

- Mail servers
- Nameservers
- SPF records
- DKIM records
- TXT records
- Public infrastructure

---

### 11. Certificate Transparency Logs

Certificate Transparency logs help discover subdomains.

Popular resources:

```text
https://crt.sh

https://transparencyreport.google.com
```

Search:

```text
%.target.com
```

This often reveals:

- Development servers
- Staging environments
- VPN gateways
- Internal naming conventions

---

### 12. Wayback Machine

The Internet Archive stores historical versions of websites.

Website:

```text
https://web.archive.org
```

Possible discoveries:

- Old admin panels
- Deprecated applications
- Previous file structures
- Archived JavaScript
- Historical contact information

---

### 13. Metadata Analysis

Metadata is hidden information stored inside files such as documents, images, PDFs, videos, and Office files.

Common metadata includes:

- Author name
- Username
- Software used
- GPS coordinates
- Device model
- Creation date
- Modification date

Metadata frequently reveals useful information during an OSINT investigation.

---

### 14. ExifTool

ExifTool is one of the most widely used metadata analysis tools.

```bash
# View metadata
exiftool image.jpg

# Analyze a PDF
exiftool report.pdf

# Analyze a Word document
exiftool document.docx

# Analyze every file in a directory
exiftool *

# Remove metadata
exiftool -all= image.jpg
```

Example output:

```text
File Name          : photo.jpg
Author             : John Smith
Software           : Adobe Photoshop
Create Date        : 2024:08:01 12:40:12
GPS Latitude       : 28.6139
GPS Longitude      : 77.2090
Camera Model Name  : Canon EOS 80D
```

---

### 15. GitHub OSINT

GitHub can reveal valuable publicly available information during security assessments.

Common searches:

```text
site:github.com target.com

site:github.com api_key

site:github.com password

site:github.com secret

site:github.com AWS_SECRET

site:github.com PRIVATE_KEY

site:github.com .env

site:github.com config.php
```

Useful findings include:

- Public repositories
- API keys
- Configuration files
- Source code
- Technology stack
- Documentation
- Commit history

---

### 16. Subdomain Enumeration

Finding subdomains helps identify additional publicly accessible assets.

Common techniques:

#### Certificate Transparency

```text
crt.sh
```

Search:

```text
%.target.com
```

---

#### Google

```text
site:*.target.com
```

---

#### theHarvester

```bash
theHarvester -d target.com -b all
```

---

#### DNS

```bash
dig target.com ANY

host target.com
```

---

### 17. OSINT Reporting

Every investigation should end with clear documentation.

Suggested report structure:

```text
Target

Scope

Public Information

Employees

Email Addresses

Subdomains

DNS Records

Technologies

Public Documents

Internet-Facing Assets

Screenshots

Observations

Recommendations
```

---

## Quick Reference Cheatsheet

```text
Google:
site:
intitle:
inurl:
filetype:
ext:
cache:
related:

Whois:
whois target.com

DNS:
dig target.com
host target.com
nslookup target.com

theHarvester:
theHarvester -d target.com -b all

Certificate Transparency:
crt.sh

Wayback Machine:
web.archive.org

Metadata:
exiftool image.jpg

Shodan:
port:80
product:Apache
os:Linux
country:IN

GitHub:
site:github.com api_key
site:github.com password
site:github.com secret
```

---

## Day 9 Submission Task

```text
Complete all TryHackMe rooms listed above.

Perform an OSINT investigation against a practice target.

Collect:

• Whois information
• DNS records
• Certificate Transparency results
• Public documents
• Metadata (if available)
• GitHub findings

Create a short investigation report.

Push everything to:

100-days-pentest/
└── Day-09/

Share your progress on Discord or Telegram.
```

---

### Bonus Challenge

```text
Choose a well-known public company.

Using only publicly available information, identify:

• Subdomains
• Mail servers
• Public documents
• Technologies used

Document your findings in Markdown.
```

---

> **End of Day 9**

**Next Up:** Day 10 — Gobuster & Directory Enumeration

---
