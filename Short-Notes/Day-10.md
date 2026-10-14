# 100-Day Pentesting Challenge
## Day 10: Passive Reconnaissance — WHOIS, nslookup & dig

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| WHOIS | Identify domain ownership and infrastructure |
| DNS with `dig` | Query DNS records and map domain infrastructure |
| `nslookup` | Perform DNS lookups across platforms |
| DNS Zone Transfer | Understand how DNS misconfigurations expose information |
| Passive Recon Methodology | Build intelligence without directly interacting with the target |

> Passive reconnaissance focuses on collecting publicly available information before any active interaction with a target.

---

## THM Rooms to Complete (In Order)

| # | Room | Link | Focus |
|---|---|---|---|
| 1 | Passive Reconnaissance | https://tryhackme.com/room/passiverecon | WHOIS, nslookup, dig, passive reconnaissance |
| 2 | DNS in Detail | https://tryhackme.com/room/dnsindetail | DNS records, resolution process, TTL |

> All rooms are available on TryHackMe.

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:15 | Read theory: Passive reconnaissance + WHOIS |
| 0:15 – 0:40 | Complete THM Room 1 |
| 0:40 – 1:20 | Study DNS records and `dig` |
| 1:20 – 2:00 | Complete THM Room 2 |

---

## Must-Know Concepts

### 1. Passive vs Active Reconnaissance

| Type | Contacts Target? | Visible to Target? | Examples |
|---|---|---|---|
| Passive Recon | No | No | WHOIS, Google, Shodan, crt.sh, DNS records |
| Active Recon | Yes | Usually | Nmap, Gobuster, Nikto, Burp Suite |

Passive reconnaissance should generally be performed before active enumeration because it provides valuable information without directly interacting with the target.

---

### 2. WHOIS — Domain Intelligence

WHOIS is a public registration database that stores ownership and registration information for domain names.

#### Common Information Returned

| Field | Description |
|---|---|
| Registrant | Domain owner |
| Organization | Registered organization |
| Email | Contact email |
| Registrar | Domain registrar |
| Name Servers | Authoritative DNS servers |
| Creation Date | Registration date |
| Expiration Date | Expiration date |
| Updated Date | Last modification |

---

#### WHOIS Commands

```bash
# Basic lookup
whois google.com

# WHOIS lookup by IP
whois 216.58.211.46

# Query a specific WHOIS server
whois -h whois.verisign-grs.com google.com

# Regional WHOIS servers
whois -h whois.arin.net 8.8.8.8
whois -h whois.ripe.net 1.1.1.1
whois -h whois.apnic.net 103.21.244.0

# Extract nameservers
whois target.com | grep -i "name server"

# Extract email
whois target.com | grep -i "email"

# Extract important dates
whois target.com | grep -i "date"
```

---

#### Online WHOIS Resources

```text
who.is
domaintools.com
whoxy.com
viewdns.info
```

---

### 3. DNS Records

DNS contains far more than simple IP address mappings.

| Record | Purpose |
|---|---|
| A | IPv4 Address |
| AAAA | IPv6 Address |
| CNAME | Alias |
| MX | Mail Server |
| TXT | Text Records |
| NS | Name Server |
| SOA | Start of Authority |
| PTR | Reverse DNS |
| SRV | Service Record |
| CAA | Certificate Authority |
| SPF | Sender Policy Framework |
| DKIM | DomainKeys Identified Mail |
| DMARC | Email Authentication Policy |

---

### 4. `dig` — Domain Information Groper

`dig` is one of the most powerful DNS analysis tools available.

#### Basic Queries

```bash
# Default A record
dig google.com

dig google.com A

# Short output
dig +short google.com

# IPv6
dig google.com AAAA

# Mail servers
dig google.com MX

dig +short google.com MX

# Name servers
dig google.com NS

# TXT records
dig google.com TXT

# SOA
dig google.com SOA

# All records
dig google.com ANY

# Reverse lookup
dig -x 8.8.8.8

dig -x 8.8.8.8 +short

# Query specific DNS servers
dig @8.8.8.8 google.com A

dig @1.1.1.1 google.com A

dig @ns1.target.com target.com A
```

---

#### Advanced `dig` Usage

```bash
# Trace DNS resolution
dig +trace google.com

# Disable recursion
dig +norecurse google.com

# Multi-line output
dig +multiline google.com

# DNSSEC
dig +dnssec google.com

# Force TCP
dig +tcp google.com ANY

# Answer section only
dig +noall +answer google.com

# Show TTL
dig +ttl google.com

# Check recursion
dig +short test.openresolver.com TXT @target_dns_server
```

---

### 5. DNS Zone Transfer (AXFR)

A DNS Zone Transfer (AXFR) allows a secondary DNS server to synchronize DNS records from the primary server.

If a DNS server is misconfigured, an attacker may retrieve every DNS record for a domain.

---

#### Step 1 — Find the Name Servers

```bash
dig target.com NS

dig +short target.com NS
```

---

#### Step 2 — Attempt a Zone Transfer

```bash
dig axfr @ns1.target.com target.com

dig axfr @ns2.target.com target.com
```

---

#### Alternative Methods

```bash
# Using host
host -t axfr target.com ns1.target.com

# Using Nmap NSE
nmap --script dns-zone-transfer \
--script-args dns-zone-transfer.domain=target.com target.com

# Using Fierce
fierce --domain target.com
```

---

#### Example Successful AXFR

```text
target.com.      IN SOA  ns1.target.com.
target.com.      IN NS   ns1.target.com.
target.com.      IN NS   ns2.target.com.
www.target.com.  IN A    203.0.113.10
mail.target.com. IN A    203.0.113.11
vpn.target.com.  IN A    203.0.113.15
dev.target.com.  IN A    10.0.0.5
admin.target.com.IN A    10.0.0.10
db.target.com.   IN A    10.0.0.20
```

> **Pentest Note:** Modern DNS servers usually restrict AXFR requests, but it is still worth checking during an authorized assessment.

---

### 6. `nslookup`

`nslookup` performs DNS lookups on both Windows and Linux.

#### Basic Usage

```bash
nslookup google.com

nslookup -type=MX google.com

nslookup -type=NS google.com

nslookup -type=TXT google.com

nslookup -type=SOA google.com

nslookup -type=ANY google.com
```

---

#### Query a Specific DNS Server

```bash
nslookup google.com 8.8.8.8

nslookup google.com 1.1.1.1
```

---

#### Reverse Lookup

```bash
nslookup 8.8.8.8
```

---

#### Interactive Mode

```bash
nslookup

> set type=MX
> google.com

> set type=NS
> google.com

> exit
```

---

### `dig` vs `nslookup`

| Tool | Best Use |
|---|---|
| `dig` | Detailed DNS analysis |
| `nslookup` | Quick cross-platform lookups |
| `host` | Simple one-line queries |

---

### 7. `host`

The `host` command provides quick DNS lookups.

```bash
host google.com

host -t MX google.com

host -t NS google.com

host -t TXT google.com

host 8.8.8.8

host -t axfr target.com ns1.target.com

host -v google.com
```

---

### 8. Passive Subdomain Enumeration

Subdomains reveal additional attack surface.

Common examples include:

- admin.target.com
- api.target.com
- dev.target.com
- staging.target.com
- vpn.target.com

---

#### Using DNS

```bash
dnsenum target.com

dnsenum --dnsserver 8.8.8.8 --enum target.com

fierce --domain target.com
```

---

#### Certificate Transparency

```text
https://crt.sh

Search:

%.target.com
```

---

#### theHarvester

```bash
theHarvester -d target.com -b all
```

---

#### Amass

```bash
amass enum -passive -d target.com

amass enum -active -d target.com
```

---

#### Assetfinder

```bash
assetfinder --subs-only target.com
```

---

#### Google

```text
site:*.target.com

site:target.com -www
```

---

### 9. SPF, DKIM & DMARC

These DNS TXT records help protect domains against email spoofing.

---

#### SPF

```bash
dig target.com TXT | grep "v=spf1"
```

---

#### DMARC

```bash
dig _dmarc.target.com TXT
```

---

#### DKIM

```bash
dig default._domainkey.target.com TXT

dig mail._domainkey.target.com TXT

dig google._domainkey.target.com TXT
```

---

### SPF Policy Reference

| Value | Meaning |
|---|---|
| `~all` | Soft Fail |
| `-all` | Hard Fail |
| `+all` | Allow All |
| No Record | No SPF Policy |

---

### DMARC Policy Reference

| Policy | Meaning |
|---|---|
| `none` | Monitoring only |
| `quarantine` | Move suspicious mail to spam |
| `reject` | Reject unauthorized email |
| No Record | No DMARC Policy |

> **Pentest Note:** Reviewing SPF, DKIM, and DMARC records helps assess an organization's email security posture during an authorized assessment.

---

### 10. Full Passive Recon Workflow

A structured reconnaissance workflow helps ensure that important information is collected in a consistent manner.

```bash
TARGET="target.com"

echo "========== WHOIS =========="
whois $TARGET | grep -E "Name Server|Registrant|Email|Creation|Expir"

echo "========== DNS RECORDS =========="
dig +short $TARGET A
dig +short $TARGET AAAA
dig +short $TARGET MX
dig +short $TARGET NS
dig +short $TARGET TXT

echo "========== NAME SERVERS =========="
NS=$(dig +short $TARGET NS)
echo $NS

echo "========== ZONE TRANSFER =========="
for ns in $NS; do
    echo "Trying $ns"
    dig axfr @$ns $TARGET
done

echo "========== REVERSE DNS =========="
IP=$(dig +short $TARGET A)
dig -x $IP +short

echo "========== SPF =========="
dig +short $TARGET TXT | grep spf

echo "========== DMARC =========="
dig +short _dmarc.$TARGET TXT

echo "========== CERTIFICATE TRANSPARENCY =========="
curl -s "https://crt.sh/?q=%.$TARGET&output=json" \
| jq -r '.[].name_value' \
| sort -u
```

---

### 11. Online Passive Recon Tools

| Tool | Purpose |
|------|---------|
| ViewDNS | WHOIS, Reverse IP, DNS history |
| DNSDumpster | DNS mapping and visualization |
| SecurityTrails | Historical DNS records |
| crt.sh | Certificate Transparency logs |
| Shodan | Internet-connected devices |
| BuiltWith | Website technology detection |
| Wappalyzer | Browser-based technology detection |
| Wayback Machine | Historical website snapshots |
| HaveIBeenPwned | Email breach lookup |
| Hunter.io | Email discovery |
| Maltego | Visual OSINT investigations |

---

### 12. Recommended Passive Recon Workflow

```text
Target Domain
      │
      ▼
WHOIS Lookup
      │
      ▼
DNS Enumeration
      │
      ▼
Certificate Transparency
      │
      ▼
Subdomain Enumeration
      │
      ▼
SPF / DKIM / DMARC Review
      │
      ▼
Technology Identification
      │
      ▼
Historical Website Review
      │
      ▼
Create Recon Report
```

---

## Quick Reference Cheatsheet

```text
WHOIS
whois target.com

DNS
dig target.com
dig +short target.com
dig target.com MX
dig target.com NS
dig target.com TXT
dig target.com SOA

Reverse DNS
dig -x <IP>

Zone Transfer
dig axfr @ns1.target.com target.com

nslookup
nslookup target.com
nslookup -type=MX target.com

host
host target.com
host -t MX target.com

Certificate Transparency
https://crt.sh

DNS Enumeration
dnsenum target.com
fierce --domain target.com
amass enum -passive -d target.com
assetfinder --subs-only target.com

Email Security
dig target.com TXT
dig _dmarc.target.com TXT
```

---

## Day 10 Submission Task

```text
Complete all TryHackMe rooms listed above.

Choose a public domain.

Collect:

• WHOIS information
• A, MX, NS and TXT records
• Reverse DNS information
• Certificate Transparency results
• Passive subdomains
• SPF and DMARC configuration

Create a Markdown report summarizing your findings.

Push everything to:

100-days-pentest/
└── Day-10/

Share your progress on Discord or Telegram.
```

---

### Bonus Challenge

```text
Create a complete passive reconnaissance report for a public company.

Include:

• WHOIS
• DNS Records
• Subdomains
• Mail Servers
• SPF / DKIM / DMARC
• Technology Stack
• Certificate Transparency Results
```

---

> **End of Day 10**

**Next Up:** Day 11 — Active Reconnaissance: traceroute, ping & curl

---
