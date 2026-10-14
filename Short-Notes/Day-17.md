# 100-Day Pentesting Challenge
## Day 17: Burp Suite — Repeater & Intruder

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| Repeater — Deep Dive | Manual testing of HTTP requests and responses |
| Intruder Attack Types | Automate payload injection and parameter testing |
| Turbo Intruder | Faster request automation for Community Edition |
| Match & Replace | Automatically modify requests before sending |
| Response Analysis | Compare responses to identify anomalies |

> Repeater is used for precise manual testing, while Intruder automates repetitive request testing. Together they form the core workflow for manual web application assessments. :contentReference[oaicite:0]{index=0}

---

## THM Resources

| # | Resource | Focus |
|---|---|---|
| 1 | Burp Suite: Repeater | Manual request testing |
| 2 | Burp Suite: Intruder | Intruder attack types |
| 3 | Burp Suite: Other Modules | Decoder, Comparer, Sequencer |
| 4 | Burp Suite: Extensions | Useful Burp extensions |

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:15 | Study Repeater features |
| 0:15 – 0:45 | Complete Repeater lab |
| 0:45 – 1:00 | Study Intruder attack types |
| 1:00 – 1:30 | Learn Intruder workflow |
| 1:30 – 1:45 | Study Turbo Intruder and Match & Replace |
| 1:45 – 2:00 | Review notes and prepare GitHub submission |

---

## Must-Know Concepts

### 1. Burp Repeater

Repeater allows you to send the same request multiple times while modifying any part of it.

Typical workflow:

```text
Capture Request

↓

Send to Repeater

↓

Modify Request

↓

Send

↓

Analyze Response

↓

Repeat
```

---

### Repeater Interface

```text
Request (Left)

↓

Edit Headers

Edit Parameters

Edit Cookies

↓

Send

↓

Response (Right)

Pretty

Raw

Hex

Render
```

---

### Response Views

| View | Purpose |
|---|---|
| Pretty | Formatted HTML, JSON and XML |
| Raw | Original HTTP response |
| Hex | Hexadecimal representation |
| Render | Browser-like HTML rendering |

---

### 2. Manual Request Testing

Typical request:

```http
GET /profile?id=5 HTTP/1.1
Host: target.com
```

Modify only one parameter at a time and compare the responses.

---

### Typical Manual Tests

#### Parameter Value

Original:

```text
id=5
```

Modified:

```text
id=6
```

---

#### Header Modification

Example:

```http
X-Forwarded-For: 127.0.0.1
```

---

#### Cookie Modification

Original:

```http
Cookie:
role=user
```

Modified:

```http
Cookie:
role=admin
```

---

#### Request Method

Original:

```http
GET
```

Modified:

```http
POST
```

---

### 3. Common Repeater Workflows

#### Authentication Testing

```text
Capture Login Request

↓

Modify Username

↓

Modify Password

↓

Compare Responses
```

---

#### Parameter Testing

```text
Capture Request

↓

Modify Parameter

↓

Observe Status Code

↓

Compare Response Length
```

---

#### Header Testing

Modify headers such as:

```http
Host

Referer

Origin

User-Agent

X-Forwarded-For
```

---

#### Cookie Testing

Modify:

```http
session

role

language

theme
```

---

### Useful Repeater Features

- Multiple tabs
- Request history
- Copy as cURL
- URL encoding
- Base64 encoding
- HTML encoding
- Raw request editing

---

### Best Practices

- Change one value at a time.
- Compare every response carefully.
- Record status codes.
- Observe response length.
- Save useful requests for documentation.

---

### 4. Burp Intruder

Intruder automates sending multiple HTTP requests using different payloads.

Typical workflow:

```text
Capture Request

↓

Send to Intruder

↓

Mark Positions

↓

Select Attack Type

↓

Load Payloads

↓

Start Attack

↓

Analyze Results
```

---

### Marking Positions

Intruder replaces selected values with payload markers.

Example:

```http
username=§admin§

password=§password§
```

Only mark parameters you intend to test.

---

## Attack Types

### Sniper

One payload list is used against one position.

```text
username=§PAYLOAD§

password=admin
```

Use for:

- Parameter fuzzing
- Username testing
- Directory discovery
- Single-value testing

---

### Battering Ram

The same payload is inserted into every marked position.

```text
username=§PAYLOAD§

password=§PAYLOAD§
```

Use for:

- Default credentials
- Matching values
- Session testing

---

### Pitchfork

Each position receives its own payload list.

```text
username=§USER§

password=§PASS§
```

Example:

```text
admin  admin123

john   password

alice  welcome123
```

Use for:

- Known username/password pairs
- Multi-parameter testing

---

### Cluster Bomb

Every possible payload combination is generated.

```text
Users

admin

john

guest

×

Passwords

admin

password

123456
```

Total requests:

```text
3 × 3 = 9
```

Use for:

- Full credential testing
- Multiple parameter combinations

---

### Attack Type Comparison

| Attack Type | Payload Lists | Typical Use |
|---|---|---|
| Sniper | 1 | Single parameter testing |
| Battering Ram | 1 | Same payload everywhere |
| Pitchfork | Multiple | Matching value pairs |
| Cluster Bomb | Multiple | Every combination |

---

### 5. Payload Types

Common payload generators:

| Payload | Purpose |
|---|---|
| Simple List | Custom wordlist |
| Runtime File | Load payloads from disk |
| Character Fuzzer | Individual characters |
| Numbers | Sequential or random numbers |
| Dates | Date generation |
| Brute Forcer | Character combinations |
| Null Payloads | Repeat identical requests |

---

### Useful Wordlists

```text
/usr/share/wordlists/

/usr/share/seclists/
```

Examples:

```text
Passwords

Usernames

Web Content

Fuzzing

Discovery
```

---

### 6. Reading Attack Results

Do not review requests randomly.

Sort by:

- Status Code
- Response Length
- Response Time

Look for anomalies.

---

### Indicators of Interesting Responses

Different:

- Status code
- Length
- Redirect
- Error message
- Response time

These often indicate that the application behaved differently.

---

### Grep Match

Highlight useful keywords automatically.

Examples:

```text
Welcome

Success

Dashboard

Admin

Invalid Password

Login Failed
```

---

### Grep Extract

Extract useful values from responses.

Examples:

- Usernames
- Session IDs
- CSRF Tokens
- API Keys

---

### 7. Turbo Intruder

Turbo Intruder is a Burp extension designed for very high-speed request execution.

Installation:

```text
Extensions

↓

BApp Store

↓

Turbo Intruder

↓

Install
```

---

### Advantages

- Faster than Community Intruder
- Handles large payload sets
- Supports Python scripting
- Suitable for high-volume request testing

---

### 8. Match & Replace

Automatically modifies requests before they leave Burp.

Examples:

Replace:

```text
role=user
```

With:

```text
role=admin
```

---

Replace:

```text
admin=false
```

With:

```text
admin=true
```

---

Automatically add header:

```http
X-Test: Burp
```

---

### 9. Comparer

Comparer identifies differences between two requests or responses.

Useful for:

- Login responses
- Authorization checks
- Cookie changes
- Token comparison
- Page comparison

Workflow:

```text
Send Request A

↓

Send Request B

↓

Comparer

↓

Highlight Differences
```

---

### Key Takeaways

- Intruder automates repetitive testing.
- Choose the correct attack type before starting.
- Analyze status codes, response length and timing.
- Turbo Intruder provides high-speed automation.
- Match & Replace modifies requests automatically.
- Comparer highlights differences between requests and responses.

---

### 10. Practical Intruder Example

Captured Request:

```http
POST /login HTTP/1.1
Host: target.com
Content-Type: application/x-www-form-urlencoded

username=admin&password=test
```

Send the request to **Intruder**.

Mark the password parameter:

```text
username=admin

password=§test§
```

Load a password wordlist and start the attack.

Review:

- Status Code
- Response Length
- Response Time

Responses that differ from the majority deserve further investigation during an authorized security assessment.

---

### Another Example

Capture:

```http
GET /profile?id=10
```

Send to Repeater first.

If repetitive parameter testing is required, send it to Intruder and fuzz only the **id** parameter.

---

### Response Comparison Workflow

```text
Capture Request

↓

Repeater

↓

Modify Parameter

↓

Compare Response

↓

Interesting?

↓

Yes

↓

Send to Intruder

↓

Automate Testing
```

---

### Typical Burp Workflow

```text
Intercept Request

↓

HTTP History

↓

Send to Repeater

↓

Manual Testing

↓

Interesting Parameter?

↓

Send to Intruder

↓

Automate Payload Testing

↓

Compare Results

↓

Document Findings
```

---

### Choosing the Correct Tool

| Situation | Tool |
|-----------|------|
| Test one request manually | Repeater |
| Compare two responses | Comparer |
| Decode Base64 or URL values | Decoder |
| Test many payloads | Intruder |
| High-speed automation | Turbo Intruder |
| Analyze session randomness | Sequencer |

---

### Best Practices

- Test manually before automating.
- Modify only one parameter at a time in Repeater.
- Save interesting requests.
- Compare response size and status codes.
- Keep detailed notes throughout the assessment.
- Restrict testing to authorized targets.

---

## Quick Reference Cheatsheet

```text
Repeater

Capture Request

↓

Send to Repeater

↓

Modify Request

↓

Send

↓

Analyze Response

Intruder Attack Types

Sniper
Single Position

Battering Ram
Same Payload Everywhere

Pitchfork
Multiple Matching Payload Lists

Cluster Bomb
Every Payload Combination

Useful Metrics

Status Code

Response Length

Response Time

Useful Burp Tabs

Proxy

HTTP History

Target

Repeater

Intruder

Decoder

Comparer

Sequencer

Extensions

Useful Shortcuts

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

## Day 17 Submission Task

```text
Complete the Burp Suite Repeater and Intruder labs.

Capture multiple HTTP requests.

Practice:

• Manual testing using Repeater
• Intruder Sniper attack
• Intruder Pitchfork attack
• Intruder Cluster Bomb attack
• Compare responses
• Decode Base64 values
• Export requests as cURL

Document:

• Interesting requests
• Parameters tested
• Response differences
• Screenshots
• Lessons learned

Save your notes in Markdown.

Push everything to:

100-days-pentest/
└── Day-17/

Include screenshots of:

• Repeater
• Intruder
• Attack Results
• Comparer
```

---

### Bonus Challenge

```text
Choose a practice web application.

Perform:

• Manual parameter testing with Repeater
• Intruder Sniper attack
• Intruder Cluster Bomb attack
• Compare successful and unsuccessful responses

Record:

• Status codes
• Response lengths
• Interesting observations

Summarize your findings in Markdown.
```

---

> **End of Day 17**

**Next Up:** **Day 18 — OWASP Top 10: Introduction & Broken Access Control**

---
