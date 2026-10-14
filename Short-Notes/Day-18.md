# 100-Day Pentesting Challenge
## Day 18: OWASP Top 10 (2021) — All Vulnerability Classes

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| All 10 OWASP Categories | Industry standard for web vulnerability classification |
| Identifying Vulnerability Classes | Build pattern recognition during web assessments |
| Quick Testing Techniques | Learn simple tests for each category |
| Real-World Impact | Understand why each issue is critical |

> The OWASP Top 10 serves as a structured framework for understanding and categorizing common web application security risks. :contentReference[oaicite:0]{index=0}

---

## THM Rooms to Complete

| # | Room | Focus |
|---|---|---|
| 1 | OWASP Top 10 2025: IAAA Failures | Learn all OWASP categories |
| 2 | OWASP Juice Shop (Preview) | Practical vulnerable web application |
| 3 | Introduction to Web Hacking | Overview of web attack surface |
| 4 | PortSwigger Web Security Academy – Access Control | Practice access control concepts |

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:10 | Review OWASP overview and A01 |
| 0:10 – 0:30 | Complete first section of THM room |
| 0:30 – 0:45 | Study Injection, Authentication and Integrity topics |
| 0:45 – 1:05 | Complete remaining OWASP categories |
| 1:05 – 1:25 | Complete Juice Shop preview |
| 1:25 – 1:45 | Complete Intro to Web Hacking |
| 1:45 – 2:00 | Complete first PortSwigger lab and document notes |

---

## Must-Know Concepts

### 1. A01 — Broken Access Control

Broken Access Control occurs when users can access resources or functionality they should not be permitted to access.

Common issues:

- Insecure Direct Object Reference (IDOR)
- Horizontal Privilege Escalation
- Vertical Privilege Escalation
- Missing Function-Level Access Control
- CORS Misconfiguration

---

### Typical Tests

Access another user's resource:

```bash
curl http://target.com/api/users/1 -H "Cookie: session=yoursession"

curl http://target.com/api/users/2 -H "Cookie: session=yoursession"
```

Attempt to access administrative endpoints:

```bash
curl http://target.com/admin

curl http://target.com/admin/users

curl http://target.com/api/admin/delete
```

Review parameter values such as user IDs or role-related fields to determine whether authorization is enforced correctly.

---

### Indicators

- Viewing another user's data
- Accessing administrator pages
- Changing another user's resources
- Missing authorization checks

---

### 2. A02 — Cryptographic Failures

Cryptographic Failures occur when sensitive information is insufficiently protected.

Common findings include:

- Plaintext passwords
- Weak password hashing
- Missing HTTPS
- Weak TLS configuration
- Hardcoded secrets
- Sensitive data exposed in URLs

---

### Useful Checks

HTTPS redirection:

```bash
curl -I http://target.com
```

TLS configuration:

```bash
nmap --script ssl-enum-ciphers -p 443 target.com
```

Search source code for exposed secrets:

```bash
curl -s http://target.com | grep -i "password\|apikey\|secret\|token"
```

---

### Password Hash Examples

Generate an MD5 hash:

```bash
echo -n "password" | md5sum
```

Hashcat example:

```bash
hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt
```

John the Ripper example:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

---

### Indicators

- HTTP instead of HTTPS
- Weak hash algorithms
- Credentials exposed in URLs
- API keys inside source code

---

### 3. A03 — Injection

Injection occurs when user input is interpreted as commands or queries.

Common types include:

- SQL Injection
- Command Injection
- LDAP Injection
- XPath Injection
- NoSQL Injection

---

### SQL Injection Examples

```sql
'

''

' OR '1'='1

' OR 1=1--

admin'--

' ORDER BY 1--

' UNION SELECT NULL--
```

---

### Command Injection Examples

```bash
; whoami

| whoami

&& whoami

`whoami`

$(whoami)
```

---

### NoSQL Injection Example

```json
{
  "username": {
    "$ne": null
  },
  "password": {
    "$ne": null
  }
}
```

---

### Indicators

- Database errors
- Unexpected server responses
- Authentication bypass
- Delayed responses during testing
- Operating system command execution

---

### Key Takeaways

- Broken Access Control is the highest-ranked OWASP category.
- Cryptographic Failures expose sensitive information.
- Injection vulnerabilities affect databases, operating systems and application logic.
- Understanding these three categories provides the foundation for practical web security testing.

---

### 4. A04 — Insecure Design

Insecure Design refers to weaknesses in the application's architecture or business logic rather than implementation bugs.

Common examples:

- Weak password reset workflows
- Missing rate limiting
- Business logic flaws
- Workflow bypass
- Insecure application design

---

### Typical Tests

Rate limiting:

```bash
for i in {1..50}; do
  curl -s -X POST http://target.com/login \
    -d "user=admin&pass=wrong$i"
done
```

Business logic:

```bash
curl -X POST http://target.com/cart/update \
-d "item=1&quantity=-1"
```

Workflow validation:

```bash
curl http://target.com/checkout/step3
```

---

### Indicators

- Unlimited login attempts
- Negative values accepted
- Payment bypass
- Missing workflow validation

---

### 5. A05 — Security Misconfiguration

Security Misconfiguration occurs when security settings are missing or improperly configured.

Common findings:

- Default credentials
- Debug pages
- Directory listing
- Missing security headers
- Public cloud storage
- Verbose error messages

---

### Useful Checks

Security headers:

```bash
curl -I https://target.com
```

Check debug pages:

```bash
curl http://target.com/phpinfo.php

curl http://target.com/debug

curl http://target.com/.env
```

Directory listing:

```bash
curl http://target.com/uploads/

curl http://target.com/backup/
```

---

### Indicators

- Stack traces
- Framework information leakage
- Public backup files
- Missing HTTP security headers

---

### 6. A06 — Vulnerable & Outdated Components

Applications using outdated libraries or software may contain publicly known vulnerabilities.

Examples:

- Old Apache versions
- Outdated PHP
- Vulnerable JavaScript libraries
- Unsupported CMS plugins

---

### Version Enumeration

Headers:

```bash
curl -I http://target.com
```

Search versions:

```bash
searchsploit apache 2.4.49

searchsploit wordpress 5.4
```

WordPress:

```bash
wpscan --url http://target.com --enumerate vp,u
```

---

### Indicators

- Outdated software
- Public CVEs
- Vulnerable plugins
- Unsupported framework versions

---

### 7. A07 — Identification & Authentication Failures

Authentication weaknesses allow attackers to bypass login or hijack user sessions.

Common issues:

- Weak passwords
- Missing rate limiting
- Predictable session tokens
- Sessions remaining valid after logout
- Credentials transmitted insecurely

---

### Authentication Testing

Password testing:

```bash
hydra -l admin \
-P /usr/share/wordlists/rockyou.txt \
http-post-form \
"/login:username=^USER^&password=^PASS^:Invalid credentials"
```

Session validation:

```bash
curl -b "session=oldsessiontoken" \
http://target.com/dashboard
```

---

### JWT Example

Original:

```text
Header.Payload.Signature
```

Review the JWT header and payload to understand how the application represents authentication data.

---

### Indicators

- Weak passwords accepted
- Unlimited authentication attempts
- Predictable sessions
- Invalid sessions remaining active
- Weak JWT configuration

---

### Key Takeaways

- Insecure Design focuses on architectural weaknesses.
- Security Misconfiguration exposes unnecessary information and services.
- Outdated Components often have publicly documented vulnerabilities.
- Authentication Failures affect login, session management and identity verification.

---

### 8. A08 — Software & Data Integrity Failures

Software and Data Integrity Failures occur when applications trust software updates or user-controlled data without proper verification.

Common examples:

- Insecure Deserialization
- Server-Side Template Injection (SSTI)
- Supply Chain Attacks
- Unsigned Software Updates
- CI/CD Pipeline Integrity Issues

---

### SSTI Detection

Simple template expressions:

```text
{{7*7}}

${7*7}

<%= 7*7 %>

#{7*7}

*{7*7}
```

If the expression is evaluated by the server instead of displayed as plain text, the application may be vulnerable.

---

### Insecure Deserialization

Example serialized object:

```php
O:4:"User":2:{
s:8:"username";
s:5:"admin";
s:4:"role";
s:4:"user";
}
```

Serialized objects should never be trusted without validation.

---

### Indicators

- Template expressions evaluated
- Modified serialized objects accepted
- Untrusted software updates
- Missing integrity verification

---

### 9. A09 — Security Logging & Monitoring Failures

Applications should generate sufficient logs to detect suspicious activity.

Important events include:

- Failed logins
- Successful logins
- Authorization failures
- Administrative actions
- Password changes
- API activity

---

### Common Weaknesses

- Missing logs
- Insufficient log detail
- No monitoring
- No alerting
- Logs stored insecurely

---

### Indicators

- Brute-force attempts not detected
- No alerts after repeated failures
- Missing audit trails
- Missing administrator activity logs

---

### 10. A10 — Server-Side Request Forgery (SSRF)

SSRF occurs when the server retrieves resources on behalf of the attacker.

Typical targets include:

- Localhost services
- Internal applications
- Cloud metadata endpoints

---

### Common Parameters

Look for parameters such as:

```text
url

image

link

callback

redirect

fetch

webhook

src
```

---

### Example Requests

```bash
curl "http://target.com/fetch?url=http://127.0.0.1/"

curl "http://target.com/fetch?url=http://localhost/"
```

Cloud metadata examples:

```text
http://169.254.169.254/latest/meta-data/

http://metadata.google.internal/

http://169.254.169.254/metadata/instance
```

---

### Indicators

- Internal services become accessible
- Metadata endpoints respond
- Unexpected server-side requests
- Internal resources exposed

---

## OWASP Top 10 Summary

| Category | Primary Focus |
|---|---|
| A01 | Broken Access Control |
| A02 | Cryptographic Failures |
| A03 | Injection |
| A04 | Insecure Design |
| A05 | Security Misconfiguration |
| A06 | Vulnerable & Outdated Components |
| A07 | Identification & Authentication Failures |
| A08 | Software & Data Integrity Failures |
| A09 | Security Logging & Monitoring Failures |
| A10 | Server-Side Request Forgery |

---

## Quick Reference Cheatsheet

```text
A01
Broken Access Control

A02
Cryptographic Failures

A03
SQL Injection
Command Injection
NoSQL Injection

A04
Business Logic
Rate Limiting

A05
Default Credentials
Debug Pages
Security Headers

A06
Version Enumeration
Searchsploit
Known CVEs

A07
Weak Passwords
Sessions
JWT

A08
SSTI
Insecure Deserialization

A09
Logging
Monitoring
Alerting

A10
SSRF
Localhost
Cloud Metadata
```

---

## Day 18 Submission Task

```text
Complete:

• OWASP Top 10 THM Room
• OWASP Juice Shop (Preview)
• Introduction to Web Hacking
• One PortSwigger Access Control Lab

Create notes for each OWASP category including:

• Definition
• Typical Indicators
• Example Tests
• Key Takeaways

Save everything in Markdown.

Push to:

100-days-pentest/
└── Day-18/

Include:

• THM completion screenshots
• PortSwigger lab screenshot
• OWASP notes
```

---

### Bonus Challenge

```text
Choose a practice web application.

Identify one example of:

• Access Control
• Authentication
• Injection
• Security Misconfiguration

Document:

• Observation
• Testing Method
• Result
• Lessons Learned
```

---

> **End of Day 18**

**Next Up:** **Day 19 — OWASP Juice Shop: Practical Web Vulnerabilities**

---
