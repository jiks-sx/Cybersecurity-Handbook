# 100-Day Pentesting Challenge
## Day 16: Burp Suite — The Basics & Proxy Setup

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| Burp Suite Architecture | Understand the tool before using it |
| Proxy Setup | Route browser traffic through Burp Suite |
| Intercepting Requests | Capture and modify HTTP requests |
| HTTP History | Discover endpoints and parameters |
| Target Scope | Focus only on authorized targets |
| Decoder & Comparer | Encode, decode and compare requests |

> Burp Suite is one of the most widely used tools for web application security testing. Understanding its workflow is essential before moving into practical web exploitation. :contentReference[oaicite:0]{index=0}

---

## Learning Resource

| # | Name | Focus |
|---|---|---|
| 1 | Master Burp Suite Like A Pro In Just 1 Hour | Burp Suite Basics |

Complete the recommended video before continuing. :contentReference[oaicite:1]{index=1}

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:15 | Install Burp Suite and configure FoxyProxy |
| 0:15 – 1:10 | Complete the recommended video |
| 1:10 – 2:00 | Study Burp Suite workflow and core tabs |

---

## Must-Know Concepts

### 1. What is Burp Suite?

Burp Suite is an intercepting proxy positioned between your browser and the target web server. It allows you to inspect, modify and replay HTTP and HTTPS requests.

```text
Normal Browsing

Browser ─────────────────────────► Web Server

--------------------------------------------

With Burp Suite

Browser ──► Burp Suite ──► Web Server
              127.0.0.1:8080

You can intercept, inspect and modify requests here.
```

---

### Burp Suite Editions

| Edition | Cost | Features |
|---|---|---|
| Community | Free | Manual testing, limited Intruder speed |
| Professional | Paid | Active Scanner, fast Intruder, extensions |
| Enterprise | Paid | Enterprise-scale automated scanning |

For this challenge, **Community Edition** is sufficient. :contentReference[oaicite:2]{index=2}

---

### 2. Installation

```bash
# Kali Linux
burpsuite &

# Install if missing
sudo apt update
sudo apt install burpsuite -y

# Launch
burpsuite
```

---

### 3. Proxy Setup

#### Configure Burp Listener

```text
Proxy

↓

Options

↓

Proxy Listeners

↓

127.0.0.1:8080
```

---

#### Configure FoxyProxy

```text
Title:
Burp Suite

Proxy Type:
HTTP

Host:
127.0.0.1

Port:
8080
```

---

#### Install Burp CA Certificate

```text
Visit:

http://burpsuite

↓

Download CA Certificate

↓

Import into Browser

↓

Trust Certificate
```

---

#### Verify Setup

```text
Enable Intercept

↓

Browse to any website

↓

Request appears inside Burp

↓

Forward Request

↓

Page Loads Successfully
```

---

### 4. Burp Suite Tabs

#### Proxy

Purpose:

- Intercept Requests
- HTTP History
- WebSockets
- Proxy Configuration

---

#### Target

Purpose:

- Site Map
- Scope Management
- Target Organization

---

#### Repeater

Purpose:

- Replay Requests
- Modify Parameters
- Manual Testing

---

#### Intruder

Purpose:

- Automated Payload Testing
- Parameter Fuzzing
- Brute Force Testing

---

#### Decoder

Purpose:

- Base64
- URL Encoding
- HTML Encoding
- Hex Conversion

---

#### Comparer

Purpose:

- Compare Requests
- Compare Responses

---

#### Sequencer

Purpose:

- Analyze Session Token Randomness

---

#### Extensions

Popular Extensions

- Logger++
- Param Miner
- JWT Editor
- Turbo Intruder
- Autorize
- Retire.js

---

### 5. Proxy — Intercept

When interception is enabled, every request pauses before reaching the server.

```text
Browser

↓

Burp Proxy

↓

Forward

↓

Server
```

Buttons:

- Forward
- Drop
- Intercept On/Off
- Action

---

### Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| Ctrl + R | Send to Repeater |
| Ctrl + I | Send to Intruder |
| Ctrl + F | Forward Request |
| Ctrl + Shift + U | URL Encode |
| Ctrl + Shift + B | Base64 Encode |

---

### Modifying Requests

Example:

```http
POST /login HTTP/1.1

username=test

password=test
```

Possible modifications during authorized testing:

```text
username=admin

password=admin
```

or

```text
username=test

password=Password123
```

---

### Key Takeaways

- Burp Suite works as an intercepting proxy.
- Correct proxy configuration is essential before testing.
- The Proxy tab captures browser traffic.
- Repeater is used for manual request testing.
- Intruder automates payload testing.
- Decoder simplifies encoding and decoding tasks.

---

### 6. HTTP History

HTTP History records every request made through Burp Suite.

It remains one of the most useful tabs during manual testing.

Navigate to:

```text
Proxy

↓

HTTP History
```

---

### Information Available

Each request includes:

- Host
- URL
- HTTP Method
- Parameters
- Status Code
- Response Length
- MIME Type
- Timestamp

---

### Typical Workflow

```text
Browse Application

↓

HTTP History

↓

Find Interesting Requests

↓

Send to Repeater

↓

Modify Request

↓

Analyze Response
```

---

### What to Look For

Prioritize requests containing:

- URL parameters
- POST data
- JSON bodies
- Authentication
- File uploads
- API endpoints

Examples:

```text
GET /profile?id=5

POST /login

POST /upload

GET /api/users

POST /api/profile
```

---

### Filtering History

Useful filters:

- Requests with parameters
- Response Code 500
- JSON responses
- HTML pages
- JavaScript files

---

### Right-Click Options

Useful actions:

- Send to Repeater
- Send to Intruder
- Send to Comparer
- Copy URL
- Copy as cURL
- Show Response

---

### 7. Target Scope

Scope ensures Burp only records traffic for authorized targets.

Navigate to:

```text
Target

↓

Scope

↓

Add
```

---

### Example Scope

```text
Protocol:
Any

Host:
target.com

Port:
Any

Path:
/.*
```

---

### Benefits

- Cleaner HTTP History
- Ignore third-party requests
- Faster manual testing
- Better organization

---

### 8. Decoder

Decoder converts data between different formats.

Supported formats include:

- Base64
- URL
- HTML
- Hex
- ASCII
- Binary
- Octal

---

### Base64 Example

Input:

```text
eyJ1c2VyIjoiYWRtaW4ifQ==
```

Decode:

```json
{"user":"admin"}
```

---

### URL Encoding

Original:

```text
<script>alert(1)</script>
```

Encoded:

```text
%3Cscript%3Ealert%281%29%3C%2Fscript%3E
```

---

### HTML Encoding

Original:

```text
<script>alert(1)</script>
```

Encoded:

```text
&lt;script&gt;alert(1)&lt;/script&gt;
```

---

### Common Uses

Decoder is useful for:

- Base64 cookies
- JWT payloads
- URL encoding
- HTML encoding
- Hex conversion

---

### 9. Practical Burp Workflow

A recommended workflow for manual testing:

```text
Start Burp

↓

Configure Browser Proxy

↓

Disable Intercept

↓

Browse Entire Application

↓

Review HTTP History

↓

Identify Interesting Requests

↓

Send to Repeater

↓

Modify Parameters

↓

Observe Responses

↓

Document Findings
```

---

### Typical Requests Worth Reviewing

Authentication

```http
POST /login
```

Profile

```http
GET /profile?id=12
```

Search

```http
GET /search?q=test
```

Upload

```http
POST /upload
```

API

```http
GET /api/users
```

---

### Repeater Workflow

```text
Capture Request

↓

Send to Repeater

↓

Modify Parameter

↓

Send Request

↓

Compare Response

↓

Repeat
```

---

### Decoder Workflow

```text
Copy Cookie

↓

Paste into Decoder

↓

Decode

↓

Modify Value

↓

Encode

↓

Replace Cookie

↓

Retest
```

---

### Key Takeaways

- HTTP History records every intercepted request.
- Scope keeps testing focused on authorized targets.
- Decoder simplifies encoding and decoding.
- Repeater enables safe manual testing.
- Document interesting requests before moving forward.

---

### 10. Practical Examples

#### Example 1 — Login Request

Captured Request:

```http
POST /login HTTP/1.1

username=test
password=test123
```

Send the request to **Repeater** and modify parameters during authorized testing to observe how the application responds.

---

#### Example 2 — URL Parameter

Original Request:

```http
GET /profile?id=10
```

Observe how the application behaves when the parameter value changes.

Example:

```http
GET /profile?id=11
```

Document any differences in the response.

---

#### Example 3 — Cookie Inspection

Request:

```http
GET /dashboard HTTP/1.1

Cookie:
session=abc123
```

Use **Decoder** if the cookie appears to contain encoded data.

---

#### Example 4 — JSON Request

```http
POST /api/profile

Content-Type: application/json

{
    "name":"user",
    "email":"user@example.com"
}
```

JSON APIs are commonly encountered during web application assessments.

---

### 11. Burp Workflow Summary

```text
Start Burp

↓

Configure Browser Proxy

↓

Install CA Certificate

↓

Browse Application

↓

Review HTTP History

↓

Identify Interesting Requests

↓

Send to Repeater

↓

Modify Request

↓

Analyze Response

↓

Document Findings
```

---

### Common Burp Tabs

| Tab | Primary Purpose |
|------|-----------------|
| Proxy | Intercept and review traffic |
| HTTP History | Review captured requests |
| Target | Organize target scope |
| Repeater | Manual request testing |
| Intruder | Automated payload testing |
| Decoder | Encode and decode data |
| Comparer | Compare requests and responses |
| Sequencer | Analyze session randomness |
| Extensions | Additional functionality |

---

### Best Practices

- Configure scope before testing.
- Save Burp projects regularly.
- Keep HTTP History organized.
- Record interesting requests.
- Compare responses after every modification.
- Document every observation.

---

## Quick Reference Cheatsheet

```text
Launch

burpsuite

Proxy

127.0.0.1:8080

Important Tabs

Proxy

Target

Repeater

Intruder

Decoder

Comparer

Sequencer

Extensions

Useful Workflow

Capture Request

↓

HTTP History

↓

Send to Repeater

↓

Modify Request

↓

Analyze Response

↓

Document Findings

Decoder

Base64

URL

HTML

Hex

JWT

Useful Browser Shortcut

F12

Useful Burp Shortcuts

Ctrl + R

Send to Repeater

Ctrl + I

Send to Intruder

Ctrl + F

Forward Request

Ctrl + Shift + U

URL Encode

Ctrl + Shift + B

Base64 Encode
```

---

## Day 16 Submission Task

```text
Complete the recommended Burp Suite training.

Configure Burp Suite with your browser.

Verify:

• Proxy configuration
• Burp CA certificate
• Browser interception

Capture and document:

• Login request
• GET request
• POST request
• Response headers
• Cookies
• Interesting parameters

Practice:

• Send requests to Repeater
• Decode Base64 values
• Encode URL data

Save everything in Markdown.

Push to:

100-days-pentest/
└── Day-16/

Include screenshots of:

• Proxy Intercept
• HTTP History
• Repeater
• Decoder
```

---

### Bonus Challenge

```text
Using a practice web application:

• Capture five different requests.
• Identify all parameters.
• Identify cookies.
• Decode one Base64 value.
• Export one request as cURL.
• Document your observations.
```

---

> **End of Day 16**

**Next Up:** **Day 17 — Burp Suite Repeater & Intruder**

---
