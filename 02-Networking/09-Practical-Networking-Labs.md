# 9. Practical Networking Labs

All labs are intended for your own machine, your own VM, or a clearly authorized environment. Do not scan, capture, or test networks merely because they are reachable.

## Lab 1 — Inspect local interfaces

Run:

```bash
ip address
ip link
ip route
ip -6 route
```

Record:
- interface names;
- which interfaces appear up;
- assigned IPv4 and IPv6 addresses;
- default route, if present.

**Questions**
1. Which interface appears to be your primary VM network interface?
2. What is the prefix length?
3. Is IPv6 enabled?
4. What can these outputs not tell you?

## Lab 2 — Inspect neighbor information

```bash
ip neigh
```

Record the state and interface of entries you understand. An empty table is possible and is not automatically an error.

**Questions**
1. What is the difference between an IP address and a link-layer address?
2. Why does a remote destination normally resolve to a gateway's link-layer address on the local network?

## Lab 3 — Test loopback and DNS

```bash
ping -c 4 127.0.0.1
getent hosts example.com
```

If available:

```bash
dig example.com A
dig example.com AAAA
```

Record results and explain why a successful loopback test does not prove internet access.

## Lab 4 — Inspect listening sockets

```bash
ss -lnt
ss -lun
```

Identify only services you understand. Do not stop services as part of this lab.

**Questions**
1. Which entries are TCP and which are UDP?
2. Does an address show a loopback-only listener or a listener on a broader interface?
3. Why is a port number alone not enough to decide whether something is vulnerable?

## Lab 5 — Inspect HTTPS response headers

```bash
curl -I https://example.com
```

Record the status code and a few non-sensitive headers. A redirect is a valid possible result.

**Questions**
1. What does the status code indicate?
2. What does HTTPS protect when TLS is correctly configured?
3. Why does a valid certificate not prove the website is trustworthy?

## Lab 6 — Wireshark introduction

If Wireshark is installed, capture a small amount of traffic on your own active interface while performing an ordinary action such as opening a public documentation page. Use a short capture duration and stop it promptly.

Try display filters:

```text
dns
tcp
icmp
tcp.port == 443
```

If you do not see a protocol, it may not have occurred during the capture or may not be visible at that capture point. Do not generate unnecessary traffic to force results.

Protect the capture file. Do not upload it publicly without inspecting it for private information.

## Lab 7 — Subnetting practice

Calculate the following for `198.51.100.77/26`:
- subnet mask;
- network address;
- broadcast address;
- conventional usable host range.

Use a subnet calculator only after attempting the calculation yourself, then compare and explain any differences.

## Troubleshooting method

When something fails, use a layered checklist:
1. Interface state and address.
2. Route and gateway.
3. Local name-resolution configuration.
4. DNS result.
5. Transport connection to the intended service.
6. TLS and application response.
7. Firewall, VPN, proxy, and service logs where authorized.

Write down what each test proves and what it does not prove.

## Lab report template

```text
Lab:
Date:
Environment:
Goal:
Commands or filters:
Expected result:
Actual result:
What I learned:
What the evidence proves:
What remains unknown:
Security/privacy note:
```
