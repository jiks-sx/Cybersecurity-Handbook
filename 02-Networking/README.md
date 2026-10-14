# Chapter 02 — Computer Networking

> **Learn • Understand • Practice • Document**

**Difficulty:** Beginner → Intermediate  
**Estimated reading time:** 6–8 hours, excluding labs  
**Prerequisites:** Chapter 00 — Computer Fundamentals; Chapter 01 — Linux  
**Environment:** Kali Linux VM, your own home lab, and Wireshark where available

## Purpose

This chapter explains how devices communicate across networks, how data moves from one application to another, and how to inspect network behavior safely. The goal is to understand the concepts behind the commands and tools—not just memorize syntax.

## Chapter map

1. [Networking Fundamentals](01-Networking-Fundamentals.md)
2. [OSI and TCP/IP Models](02-OSI-and-TCPIP-Models.md)
3. [MAC, IP, IPv4, IPv6, and Subnetting](03-MAC-IP-and-Subnetting.md)
4. [TCP, UDP, Ports, and Sockets](04-TCP-UDP-Ports-and-Sockets.md)
5. [ARP, ICMP, DHCP, and DNS](05-ARP-ICMP-DHCP-and-DNS.md)
6. [Switching, Routing, NAT, and VLANs](06-Switching-Routing-NAT-and-VLANs.md)
7. [Web Protocols, TLS, and Common Services](07-Web-Protocols-TLS-and-Services.md)
8. [Network Security and Traffic Inspection](08-Network-Security-and-Traffic-Inspection.md)
9. [Practical Networking Labs](09-Practical-Networking-Labs.md)
10. [Revision, Interview Questions, and Cheat Sheet](10-Revision-Interview-and-Cheat-Sheet.md)

## How to study

1. Read the explanation first.
2. Draw the packet flow in your own notes.
3. Predict what a command should show before running it.
4. Run labs only on your own system or an explicitly authorized environment.
5. Record actual output and explain it in your own words.
6. Review the limits of each test. One successful or failed command rarely proves the whole network is working or broken.

## What you should be able to do after this chapter

- Explain how data moves between applications on different devices.
- Describe the OSI and TCP/IP models and their practical limits.
- Understand MAC addresses, IP addresses, prefixes, gateways, and routes.
- Explain TCP handshakes, UDP datagrams, ports, and sockets.
- Describe the roles of ARP, ICMP, DHCP, and DNS.
- Understand switching, routing, NAT, and VLANs at a foundational level.
- Explain HTTP, HTTPS, certificates, and TLS at a high level.
- Inspect your own Linux network configuration and capture authorized traffic.
- Troubleshoot common connectivity problems in a logical order.

## Scope and safety

Networking is a broad field. This chapter establishes a strong practical foundation but is not a complete replacement for vendor documentation, protocol standards, or advanced network engineering references. Addresses, commands, and outputs vary by operating system and configuration.

Only capture traffic you are authorized to inspect. Packet captures may include usernames, cookies, internal addresses, device names, and other sensitive information. Do not publish raw captures without reviewing them.

## Completion checklist

- [ ] I can explain packet encapsulation and decapsulation.
- [ ] I can distinguish MAC addresses from IP addresses.
- [ ] I understand IP prefixes and the purpose of a default gateway.
- [ ] I can explain TCP vs UDP.
- [ ] I can explain ARP, ICMP, DHCP, and DNS.
- [ ] I can inspect interfaces, routes, and listening sockets on Linux.
- [ ] I can explain NAT, switching, routing, and VLANs.
- [ ] I understand why HTTPS uses TLS and what a certificate does.
- [ ] I completed the safe labs and recorded observations.
