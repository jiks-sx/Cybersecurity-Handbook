# 100-Day Pentesting Challenge
## Phase 2: Web Application Basics & OWASP Top 10
### Day 15: Web Fundamentals — HTTP, Cookies & Sessions

> **Platform:** TryHackMe (THM) &nbsp;|&nbsp; **Difficulty:** Easy &nbsp;|&nbsp; **Time:** 2+ Hours

---

## What You'll Learn Today

| Topic | Why It Matters |
|---|---|
| HTTP Request/Response Cycle | Every web attack is a manipulated HTTP request |
| HTTP Methods & Status Codes | Understand how clients communicate with servers |
| HTTP Headers | Headers contain authentication, cookies, version info and security policies |
| Browser Requests | Learn how browsers communicate with web servers |

> Every web vulnerability in Phase 2 begins with understanding HTTP. Mastering requests and responses is the foundation of web application penetration testing.

---

## THM Room to Complete

| # | Room | Focus |
|---|---|---|
| 1 | Web Application Basics | HTTP methods, headers, status codes |

---

## 2-Hour Schedule

| Time | Activity |
|---|---|
| 0:00 – 0:15 | Study HTTP request/response cycle |
| 0:15 – 1:00 | Complete THM room |
| 1:00 – 1:40 | Study headers, cookies and sessions |
| 1:40 – 2:00 | Review notes and prepare GitHub submission |

---

## Must-Know Concepts

### 1. The HTTP Request / Response Cycle

Every time you visit a website, your browser performs several steps before displaying the page.

```text
Browser
   │
   │ DNS Lookup
   ▼
DNS Server
   │
   ▼
Web Server
   │
   │ TCP Handshake
   ▼
TLS Handshake (HTTPS)
   │
   ▼
HTTP Request
   │
   ▼
HTTP Response
   │
   ▼
Browser Renders Page
```

---

### Example HTTP Request

```http
GET /login HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Cookie: session=abc123
```

---

### Example HTTP Response

```http
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: session=xyz789

<html>
...
</html>
```

---

### Request Flow

1. Browser performs DNS lookup.
2. TCP connection is established.
3. TLS handshake occurs (HTTPS only).
4. Browser sends an HTTP request.
5. Server processes the request.
6. Server returns an HTTP response.
7. Browser renders the content.

---

### 2. HTTP Methods

| Method | Purpose | Common Pentesting Use |
|---|---|---|
| GET | Retrieve data | Parameter testing |
| POST | Submit data | Login forms, APIs |
| PUT | Replace or upload resources | File upload testing |
| PATCH | Modify existing resources | API testing |
| DELETE | Remove resources | Authorization testing |
| HEAD | Retrieve headers only | Reconnaissance |
| OPTIONS | Show supported methods | Server enumeration |
| TRACE | Echo request | Cross-Site Tracing testing |
| CONNECT | Create HTTP tunnel | Proxy communication |

---

### Checking Allowed Methods

```bash
curl -X OPTIONS http://target.com -v

nmap --script http-methods -p 80 target.com
```

---

### 3. HTTP Status Codes

#### Success (2xx)

| Code | Meaning |
|---|---|
| 200 | Request successful |
| 201 | Resource created |
| 204 | Success with no content |

---

#### Redirects (3xx)

| Code | Meaning |
|---|---|
| 301 | Permanent redirect |
| 302 | Temporary redirect |
| 307 | Temporary redirect preserving method |

---

#### Client Errors (4xx)

| Code | Meaning |
|---|---|
| 400 | Bad request |
| 401 | Authentication required |
| 403 | Forbidden |
| 404 | Resource not found |
| 405 | Method not allowed |
| 429 | Too many requests |

---

#### Server Errors (5xx)

| Code | Meaning |
|---|---|
| 500 | Internal server error |
| 502 | Bad gateway |
| 503 | Service unavailable |

---

### Understanding Status Codes

- **2xx** indicates success.
- **3xx** indicates redirection.
- **4xx** indicates client-side errors.
- **5xx** indicates server-side failures.

Always pay attention to unexpected status codes during reconnaissance.

---

### 4. Common 403 Bypass Techniques

Different web applications handle requests differently.

Examples to test during an **authorized** security assessment:

```bash
# Change request method
curl -X POST http://target.com/admin

curl -X HEAD http://target.com/admin

# Path variations
curl http://target.com/admin/

curl http://target.com//admin

curl http://target.com/./admin

# Custom headers
curl -H "X-Original-URL: /admin" http://target.com/

curl -H "X-Rewrite-URL: /admin" http://target.com/

curl -H "X-Forwarded-For: 127.0.0.1" http://target.com/admin

curl -H "Referer: http://target.com/admin" http://target.com/admin
```

---

### Key Takeaways

- HTTP is the foundation of web application security.
- Every request consists of methods, headers and optional body data.
- Every response includes status codes, headers and content.
- Understanding HTTP behavior is essential before learning Burp Suite and OWASP vulnerabilities.

---

### 5. HTTP Headers

HTTP headers carry additional information between the client and the server.

They reveal:

- Authentication
- Cookies
- Server software
- Backend technology
- Security policies
- Content type

---

### Request Headers

Headers sent by the client.

| Header | Purpose | Pentesting Use |
|---|---|---|
| `Host` | Target hostname | Host header testing |
| `User-Agent` | Browser identification | User-Agent spoofing |
| `Referer` | Previous page | Referer validation testing |
| `Cookie` | Session information | Session hijacking analysis |
| `Authorization` | Authentication token | API authentication testing |
| `Content-Type` | Request body format | Input validation testing |
| `X-Forwarded-For` | Original client IP | Proxy/IP restriction testing |
| `X-Real-IP` | Client IP | Header manipulation testing |
| `Origin` | Request origin | CORS testing |
| `Accept` | Accepted response types | Content negotiation |
| `Accept-Language` | Preferred language | Localization testing |

---

### Response Headers

Headers returned by the server.

| Header | Purpose |
|---|---|
| `Server` | Server software and version |
| `X-Powered-By` | Backend technology |
| `Set-Cookie` | Creates session cookies |
| `Content-Security-Policy` | Browser security policy |
| `X-Frame-Options` | Clickjacking protection |
| `Access-Control-Allow-Origin` | CORS policy |
| `Strict-Transport-Security` | HTTPS enforcement |
| `X-Content-Type-Options` | MIME protection |

---

### View Headers

```bash
curl -I http://target.com

curl -I https://target.com

curl -v http://target.com
```

---

### 6. Cookies

Cookies are small pieces of data stored by the browser and sent with future requests.

Most authentication systems rely on cookies to identify users.

---

### Cookie Example

```http
Set-Cookie: sessionid=abc123;
HttpOnly;
Secure;
SameSite=Lax;
Path=/
```

---

### Cookie Attributes

| Attribute | Purpose |
|---|---|
| HttpOnly | Prevent JavaScript access |
| Secure | Send only over HTTPS |
| SameSite | Helps reduce CSRF risk |
| Domain | Defines valid domain |
| Path | Limits where cookie is sent |
| Expires | Expiration date |
| Max-Age | Lifetime in seconds |

---

### Cookie Security

| Missing Attribute | Potential Risk |
|---|---|
| HttpOnly | JavaScript may access cookies |
| Secure | Cookie may be sent over HTTP |
| SameSite | Cross-site request risks increase |

---

### Base64 Cookie Example

Decode:

```bash
echo "eyJ1c2VybmFtZSI6InVzZXIifQ==" | base64 -d
```

Encode:

```bash
echo '{"username":"admin"}' | base64
```

---

### 7. Sessions

After successful authentication, the server creates a session and associates it with the user.

Typical workflow:

```text
Login Request
      │
      ▼
Credentials Verified
      │
      ▼
Session Created
      │
      ▼
Cookie Returned
      │
      ▼
Browser Stores Cookie
      │
      ▼
Future Requests Include Cookie
```

---

### Session Example

Login:

```http
POST /login

username=user
password=password
```

Server response:

```http
HTTP/1.1 200 OK

Set-Cookie:
session=abc123
```

Subsequent request:

```http
GET /dashboard

Cookie:
session=abc123
```

---

### Session vs JWT

| Feature | Session | JWT |
|---|---|---|
| Storage | Server | Client |
| Format | Random identifier | Encoded JSON |
| Validation | Server lookup | Signature verification |
| Typical Use | Traditional web apps | APIs |

---

### JWT Structure

```text
Header

.

Payload

.

Signature
```

Example:

```text
xxxxx.yyyyy.zzzzz
```

---

### Decode JWT Sections

```bash
echo "HEADER" | base64 -d

echo "PAYLOAD" | base64 -d
```

---

### 8. Same-Origin Policy (SOP)

The Same-Origin Policy prevents JavaScript running on one origin from freely reading resources belonging to another origin.

An origin consists of:

- Protocol
- Host
- Port

---

### Same Origin Examples

| URL A | URL B | Same Origin |
|---|---|---|
| https://example.com | https://example.com | Yes |
| https://example.com | http://example.com | No |
| https://example.com | https://api.example.com | No |
| https://example.com | https://example.com:8443 | No |

---

### Cross-Origin Resource Sharing (CORS)

CORS allows servers to selectively permit cross-origin requests.

Example request:

```http
Origin: https://example.org
```

Example response:

```http
Access-Control-Allow-Origin:
https://example.org
```

---

### Testing CORS

```bash
curl \
-H "Origin: https://example.org" \
-I https://target.com
```

Another example:

```bash
curl \
-H "Origin: null" \
-I https://target.com
```

---

### Key Takeaways

- Headers reveal valuable information during reconnaissance.
- Cookies maintain authenticated sessions.
- Sessions identify users across requests.
- JWTs are commonly used for API authentication.
- SOP restricts browser access across different origins.
- CORS defines when cross-origin requests are permitted.

---

### 9. Browser Developer Tools

Every modern browser includes Developer Tools.

Open them with:

```text
F12

or

Ctrl + Shift + I
```

Developer Tools are one of the most important resources for web application testing.

---

### Network Tab

Monitor every HTTP request made by the browser.

Useful information:

- Request URL
- Request Method
- Request Headers
- Response Headers
- Cookies
- POST Data
- JSON Responses
- Response Codes

You can also:

- Replay requests
- Copy requests as cURL
- Inspect API endpoints

---

### Application Tab

Useful storage locations:

```text
Cookies

Local Storage

Session Storage

IndexedDB
```

Look for:

- Session IDs
- JWT tokens
- API keys
- User preferences

---

### Console

Execute JavaScript directly inside the page.

Examples:

```javascript
document.cookie

localStorage

sessionStorage

document.domain

window.location.href
```

---

### Sources

Review JavaScript loaded by the application.

Search for:

- API endpoints
- API keys
- Hidden routes
- Comments
- Hardcoded credentials
- Disabled functionality

---

### Elements

Inspect and modify HTML.

Examples:

- Edit form fields
- Remove disabled attributes
- Reveal hidden inputs
- Change password fields to text
- Test client-side validation

---

### 10. URL Structure

Example:

```text
https://shop.target.com:8443/products/search?q=phone&page=1

│
├── Protocol
├── Host
├── Port
├── Path
└── Parameters
```

---

### URL Components

| Component | Example |
|---|---|
| Protocol | HTTPS |
| Host | shop.target.com |
| Port | 8443 |
| Path | /products/search |
| Parameters | q=phone&page=1 |

---

### Common URL Encoding

| Character | Encoding |
|---|---|
| Space | `%20` |
| `/` | `%2F` |
| `?` | `%3F` |
| `&` | `%26` |
| `=` | `%3D` |
| `'` | `%27` |
| `"` | `%22` |
| `<` | `%3C` |
| `>` | `%3E` |

---

### 11. Manual Web Enumeration Methodology

Always inspect the application manually before using automated tools.

Recommended workflow:

```text
Browse Application

↓

Identify Pages

↓

Inspect Requests

↓

Review Headers

↓

Inspect Cookies

↓

View Source Code

↓

Check robots.txt

↓

Review JavaScript Files

↓

Identify Parameters

↓

Begin Testing
```

---

### Manual Checklist

Review:

- Navigation
- Login page
- Registration
- Search functionality
- File uploads
- User profile
- API endpoints
- Error messages
- Hidden parameters

---

### Useful Commands

Headers:

```bash
curl -I http://target.com
```

robots.txt

```bash
curl http://target.com/robots.txt
```

View source:

```bash
curl http://target.com
```

Check JavaScript:

```bash
curl http://target.com/app.js
```

---

## Quick Reference Cheatsheet

```text
HTTP Methods

GET
POST
PUT
PATCH
DELETE
HEAD
OPTIONS

Status Codes

200
301
302
401
403
404
500

Headers

Host
Cookie
Authorization
Content-Type
Origin
User-Agent
Server
Set-Cookie

Cookies

HttpOnly
Secure
SameSite

Sessions

Server-side Session

JWT

Header.Payload.Signature

Developer Tools

Network
Application
Console
Sources
Elements

Useful curl Commands

curl -I target

curl -v target

curl -X OPTIONS target

curl -H "Origin: https://example.org" target

Manual Enumeration

Browse

Inspect Requests

Review Headers

Review Cookies

Check robots.txt

Inspect JavaScript

Identify Parameters
```

---

## Day 15 Submission Task

```text
Complete the TryHackMe room.

Using any web application:

• Capture HTTP requests using Browser Developer Tools.
• Identify request headers.
• Identify response headers.
• Identify cookies.
• View robots.txt.
• Identify server technology.
• Inspect JavaScript files.

Document:

• Interesting HTTP headers
• Cookies discovered
• Server software
• Backend technology
• Manual observations

Push everything to:

100-days-pentest/
└── Day-15/

Include screenshots of:

• Network Tab
• Application Tab
• HTTP Headers
• robots.txt
```

---

### Bonus Challenge

```text
Choose a web application lab.

Identify:

• Session Cookie
• Response Headers
• JavaScript Files
• API Endpoint
• robots.txt

Document your findings in Markdown.
```

---

> **End of Day 15**

**Next Up:** **Day 16 — Burp Suite: The Basics**

---
