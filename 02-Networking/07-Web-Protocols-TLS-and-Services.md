# 7. Web Protocols, TLS, and Common Services


## 7.1 HTTP

Hypertext Transfer Protocol (HTTP) defines how clients and servers exchange requests and responses. A request can include a method, path, headers, and optional body. A response includes a status code, headers, and optional body.

Common methods include:
- `GET`: request a representation;
- `POST`: submit data or request processing;
- `PUT`: create or replace a resource representation, depending on API semantics;
- `PATCH`: partially modify a resource;
- `DELETE`: request deletion of a resource.

Actual behavior depends on the application and API. A method name alone does not prove what a server will do.

## 7.2 HTTP status codes

- `2xx`: success class;
- `3xx`: redirection class;
- `4xx`: client/request-related error class;
- `5xx`: server-error class.

Examples: `200 OK`, `301 Moved Permanently`, `400 Bad Request`, `401 Unauthorized` (authentication is required or failed, depending on context), `403 Forbidden`, `404 Not Found`, `500 Internal Server Error`.

Status codes are clues. The response body, headers, server logs, and application behavior may be needed to understand the issue.

## 7.3 HTTPS and TLS

HTTPS is HTTP carried over a TLS-protected connection. TLS can provide confidentiality, integrity, and server authentication when correctly configured and validated. Client certificates can also be used in some deployments.

In a simplified TLS setup:
1. Client and server negotiate supported parameters.
2. The server presents a certificate chain.
3. The client validates the certificate for the intended hostname and trust chain.
4. Key agreement establishes shared secrets.
5. Encrypted application data is exchanged.

Modern TLS versions have details that differ from older handshake descriptions. Do not assume every TLS connection uses the same messages or certificate configuration.

## 7.4 What a certificate does—and does not do

A certificate binds a public key to an identity according to a certificate authority's validation process. Correct hostname and trust-chain validation are important. A valid certificate does not prove a website is honest, free of malware, or safe to use; phishing websites can also obtain valid certificates.

## 7.5 Inspect a response

```bash
curl -I https://example.com
```

This requests response headers, where supported by the server and `curl` behavior. A server may redirect or respond differently depending on configuration. Do not send credentials or private data to a site merely to test a command.

For a verbose connection trace:

```bash
curl -vI https://example.com
```

Verbose output can reveal addresses, headers, certificate information, and other metadata. Review it before sharing.

## 7.6 Other common protocols

- **SSH:** Encrypted remote administration and secure file transfer features.
- **SMTP:** Email transfer between clients and mail systems or between mail servers.
- **IMAP/POP3:** Mail access protocols.
- **NTP:** Time synchronization.
- **SMB:** File and printer sharing, commonly in Windows-oriented environments.
- **RDP:** Remote desktop protocol commonly associated with Windows.
- **MySQL protocol:** Communication with MySQL-compatible database services.

Default ports are conventions and can be changed. Protocol support and security depend on implementation and configuration.

## 7.7 Security basics

- Use HTTPS and validate certificates.
- Avoid transmitting passwords over unencrypted protocols.
- Restrict remote administration to intended networks and users.
- Keep exposed services patched.
- Disable unnecessary services.
- Do not expose databases or management interfaces to the public internet without a justified, secured design.
- Remember that encryption protects traffic in transit, not a compromised endpoint.

## 7.8 Summary

HTTP defines web requests and responses; TLS can protect the connection and authenticate the server's identity. A certificate is not a guarantee that a site is trustworthy. Understand protocol purpose, configuration, and exposure before assessing risk.
