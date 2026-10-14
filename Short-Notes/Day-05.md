# 100-Day Pentesting Challenge
## Day 5: How The Web Works

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** ~1 Hour

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| HTTP & HTTPS | Every web attack lives inside HTTP requests |
| DNS | How domains resolve and how attackers enumerate them |
| URLs | Learn to identify potential attack points by reading URLs |
| How Web Requests Work | Understand the complete browser-to-server communication flow |

> **Web applications are the primary attack surface in bug bounty hunting and penetration testing.**
> You cannot exploit what you don't understand. Today you'll learn how the web works behind the scenes.

---

## THM Rooms to Complete (In Order)

| # | Room | Link | Focus |
|---|---|---|---|
| 1 | DNS in Detail | https://tryhackme.com/room/dnsindetail | DNS records, resolution, zone transfer |
| 2 | HTTP in Detail | https://tryhackme.com/room/httpindetail | HTTP methods, status codes, headers, cookies |
| 3 | Networking Devices & Initial Config | Search **"Skills For All Cisco Networking Devices"** on https://skillsforall.com | Routers, switches, NAT, VLANs |

> All rooms are part of the **Pre-Security Path** on TryHackMe and Cisco SkillsForAll.

---

## 1-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:10 | Read the theory below (HTTP + DNS quick reference) |
| 0:10 – 0:25 | Complete THM Room 1 — DNS in Detail |
| 0:25 – 0:45 | Complete THM Room 2 — HTTP in Detail |
| 0:45 – 0:55 | Complete Cisco Networking Devices module |
| 0:55 – 1:00 | Review the cheatsheet and update GitHub |

---

## Must-Know Concepts

### 1. What Happens When You Visit a Website

```text
You type: https://google.com
           │
           ▼
   [1] DNS Lookup
       Browser asks:
       "What is the IP address of google.com?"

       DNS Server replies:
       "142.250.80.36"

           │
           ▼
   [2] TCP Connection
       (3-Way Handshake on Port 443)

           │
           ▼
   [3] TLS Handshake
       Encrypts the connection

           │
           ▼
   [4] HTTP Request
       GET / HTTP/1.1
       Host: google.com

           │
           ▼
   [5] HTTP Response
       HTML + CSS + JavaScript
       Browser renders the page
```

> **Pentest Note:** Tools like Burp Suite intercept traffic during the HTTP request stage, allowing requests to be inspected and modified.

---

### 2. HTTP Methods

| Method | Purpose | Security Relevance |
|---|---|---|
| `GET` | Retrieve data | URL parameters can often be manipulated |
| `POST` | Submit data | Used by forms, APIs, authentication |
| `PUT` | Upload or replace resources | Can expose file upload vulnerabilities |
| `DELETE` | Remove resources | Poor authorization may allow unauthorized deletion |
| `PATCH` | Modify existing resources | Frequently tested for authorization issues |
| `HEAD` | Return headers only | Useful during reconnaissance |
| `OPTIONS` | Show supported methods | Reveals server capabilities |

---

### 3. HTTP Status Codes

| Code | Meaning | Pentesting Relevance |
|---|---|---|
| `200` | OK | Request succeeded |
| `201` | Created | Resource successfully created |
| `301` | Moved Permanently | Permanent redirect |
| `302` | Found | Temporary redirect |
| `400` | Bad Request | May reveal validation behavior |
| `401` | Unauthorized | Authentication required |
| `403` | Forbidden | Access denied |
| `404` | Not Found | Useful during directory enumeration |
| `405` | Method Not Allowed | Test alternative HTTP methods |
| `500` | Internal Server Error | May reveal application details |
| `503` | Service Unavailable | Service temporarily unavailable |

> **Note:** Different responses to different HTTP methods or paths can provide useful information during application assessment.

---

### 4. HTTP Headers

#### Request Headers

| Header | Purpose | Security Relevance |
|---|---|---|
| `Host` | Target domain | Important for virtual hosts |
| `User-Agent` | Client identifier | Can sometimes be modified during testing |
| `Cookie` | Session identifier | Session management testing |
| `Authorization` | Authentication credentials | Token-based authentication |
| `Referer` | Previous page | Can occasionally influence application behavior |
| `Content-Type` | Body format | Important during file uploads and APIs |
| `X-Forwarded-For` | Original client IP | Common in proxy environments |

#### Response Headers

| Header | Purpose | Security Relevance |
|---|---|---|
| `Set-Cookie` | Creates cookies | Check security flags |
| `Server` | Web server information | Technology fingerprinting |
| `X-Powered-By` | Backend technology | Version enumeration |
| `Content-Security-Policy` | Browser security policy | Important for XSS mitigation |
| `Access-Control-Allow-Origin` | CORS configuration | Cross-origin access control |
| `Location` | Redirect destination | Redirect behavior |

---

### 5. DNS Record Types

| Record | Purpose | Pentesting Use |
|---|---|---|
| `A` | Domain → IPv4 | Identify server IP |
| `AAAA` | Domain → IPv6 | IPv6 enumeration |
| `CNAME` | Alias | Discover related domains |
| `MX` | Mail servers | Email infrastructure |
| `TXT` | Text records | SPF, DKIM, verification records |
| `NS` | Name servers | DNS infrastructure |
| `PTR` | Reverse DNS | IP-to-hostname mapping |

**Useful DNS Commands**

```bash
dig google.com A
dig google.com MX
dig google.com NS
dig axfr @ns1.target.com target.com
dig +short google.com
nslookup google.com
host -t TXT google.com
```

---

### 6. Anatomy of a URL

```text
https://shop.target.com:8443/products/search?q=shoes&sort=asc#results
  │         │           │        │              │              │
scheme   subdomain    port     path         parameters     fragment
```

| Component | Description |
|---|---|
| Scheme | `http` or `https` |
| Subdomain | Example: `shop` |
| Port | Example: `8443` |
| Path | Requested resource |
| Parameters | Query values sent to the server |
| Fragment | Client-side anchor (not sent to the server) |

> URL parameters are common locations where user input is processed and should be carefully tested during security assessments.

---

### 7. Cookies & Security Flags

```text
Set-Cookie:
sessionid=abc123;
HttpOnly;
Secure;
SameSite=Strict;
Path=/
```

| Flag | Purpose | Benefit |
|---|---|---|
| `HttpOnly` | Prevents JavaScript access | Helps reduce session theft via XSS |
| `Secure` | Sent only over HTTPS | Protects against interception |
| `SameSite` | Restricts cross-site requests | Helps mitigate CSRF attacks |

---

## Quick Reference Cheatsheet

```text
HTTP Methods:
GET  POST  PUT  DELETE  PATCH  OPTIONS

Status Codes:
200 301 302 400 401 403 404 405 500 503

Important Headers:
Host
Cookie
Authorization
Content-Type
User-Agent
X-Forwarded-For

DNS Records:
A
AAAA
CNAME
MX
TXT
NS
PTR

Useful DNS:
dig
nslookup
host

Cookie Flags:
HttpOnly
Secure
SameSite
```

---

## Day 5 Submission Task

```text
Complete all learning modules listed above

Take screenshots showing completion

Push screenshots to:
100-days-pentesting/Day-05/

Share your daily progress on Discord or Telegram
```

---

> **End of Day 5**

**Next Up:** Day 6 — Introduction to Networking: Ports, Protocols & Wireshark

---
