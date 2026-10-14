# 10. Revision, Interview Questions, and Cheat Sheet

## 10.1 Quick revision

- A network connects devices so they can exchange data.
- Frames are used on a local link; IP packets are forwarded between networks.
- A switch commonly forwards Ethernet frames; a router forwards IP packets.
- MAC addresses and IP addresses serve different purposes.
- Prefix length defines the network portion of an IP address.
- A default gateway is a next hop for destinations without a more specific route.
- TCP provides a reliable ordered byte stream; UDP provides datagrams.
- Ports help identify transport endpoints, but do not prove which service is running.
- ARP commonly resolves IPv4-to-link-layer mappings on a local link.
- ICMP provides network control and diagnostic messages.
- DHCP distributes network configuration.
- DNS stores and retrieves records associated with names.
- NAT translates address information; it is not the same as a firewall.
- VLANs create logical Layer 2 separation, but policy must control cross-VLAN traffic.
- HTTPS uses TLS for protected HTTP communication when configured and validated correctly.
- Packet captures are point-in-time evidence and may contain sensitive information.

## 10.2 Command cheat sheet

| Goal | Command |
|---|---|
| Show interfaces and addresses | `ip address` |
| Show link state | `ip link` |
| Show IPv4 routes | `ip route` |
| Show IPv6 routes | `ip -6 route` |
| Show neighbor cache | `ip neigh` |
| Test local IP stack | `ping -c 4 127.0.0.1` |
| Query system name resolution | `getent hosts example.com` |
| Query DNS A record, if `dig` exists | `dig example.com A` |
| Inspect listening TCP sockets | `ss -lnt` |
| Inspect listening UDP sockets | `ss -lun` |
| Inspect sockets with process details | `ss -lntup` |
| Request HTTPS response headers | `curl -I https://example.com` |
| Inspect kernel version | `uname -r` |
| Inspect system time | `date` |

Check manual pages and command help for exact syntax. Do not share output before reviewing it for sensitive information.

## 10.3 Interview questions — beginner

1. What is a computer network?
2. What is the difference between a switch and a router?
3. What is the difference between a MAC address and an IP address?
4. What are the OSI model's seven layers?
5. What is a default gateway?
6. What is the difference between bandwidth and throughput?
7. What is the purpose of DNS?
8. What is DHCP used for?
9. What is ICMP?
10. What is the difference between TCP and UDP?

## 10.4 Interview questions — intermediate

11. Explain a TCP three-way handshake.
12. What happens when a device sends traffic to an IP address outside its subnet?
13. Why does a router normally rewrite link-layer framing when forwarding a packet?
14. What does `/24` mean in IPv4 notation?
15. What is NAT, and why is it not a replacement for a firewall?
16. What is a VLAN?
17. What does a listening socket tell you?
18. Why does a failed ping not prove a host is offline?
19. What does TLS provide, and what does a certificate validate?
20. What limitations should you remember when analyzing a packet capture?

## 10.5 Short answer key

1. A network connects devices for data exchange.
2. A switch commonly forwards local-link frames; a router forwards IP packets between networks.
3. MAC addresses support link-layer delivery; IP addresses support logical addressing and routing.
4. Physical, Data Link, Network, Transport, Session, Presentation, Application.
5. A next hop used when no more specific route matches.
6. Bandwidth is capacity; throughput is observed successful transfer rate.
7. DNS retrieves records associated with domain names.
8. DHCP can distribute IP configuration such as address, gateway, and DNS server.
9. ICMP carries network control and error/diagnostic messages.
10. TCP provides a reliable ordered byte stream; UDP provides datagrams without TCP's delivery guarantees.
11. SYN, SYN-ACK, ACK in the common connection-establishment case.
12. The host usually selects a route and sends a frame to its next-hop gateway.
13. The next link needs its own link-layer source and destination addresses.
14. The first 24 bits are the network prefix.
15. NAT translates address information; firewall policy separately controls allowed traffic.
16. A logical Layer 2 broadcast domain.
17. A process is waiting for traffic on an endpoint; it does not by itself prove vulnerability.
18. ICMP may be blocked and other protocols may still work.
19. TLS can protect confidentiality/integrity and authenticate the server when validation succeeds; certificates bind keys to identities under a trust process.
20. Captures can be incomplete, point-specific, encrypted, and privacy-sensitive.

## 10.6 Scenario practice

1. Your laptop can reach local devices but not the internet. What do you inspect first?
2. A domain fails to resolve but a known IP works. Which possibilities would you investigate?
3. A TCP port is listening on `0.0.0.0`. What does that suggest, and what information is still missing?
4. A ping fails but HTTPS works. How can both results be true?
5. Wireshark shows TLS traffic but not readable HTTP content. Why?
6. Two devices have addresses in different subnets. What normally enables them to communicate?
7. A guest VLAN can access a management server. Which design and policy areas would you review?

## 10.7 Further study

- IETF RFC index: https://www.rfc-editor.org/
- IPv4 specification (RFC 791): https://www.rfc-editor.org/rfc/rfc791
- IPv6 specification (RFC 8200): https://www.rfc-editor.org/rfc/rfc8200
- TCP specification (RFC 9293): https://www.rfc-editor.org/rfc/rfc9293
- DNS concepts (RFC 1034): https://www.rfc-editor.org/rfc/rfc1034
- DNS implementation and specification (RFC 1035): https://www.rfc-editor.org/rfc/rfc1035
- Wireshark User's Guide: https://www.wireshark.org/docs/wsug_html_chunked/
- Linux `ip` manual: `man ip`
- Linux `ss` manual: `man ss`
- Kali Linux documentation: https://www.kali.org/docs/

## 10.8 Final self-check

Without looking at your notes, explain:
- how a packet travels from your device to a remote server;
- how MAC addresses differ from IP addresses;
- how to identify your default route;
- why TCP and UDP are different;
- what ARP, ICMP, DHCP, and DNS do;
- why NAT and firewalling are not identical;
- how HTTPS uses TLS;
- how to inspect your own network safely.

If you cannot explain a topic clearly, return to the relevant chapter and repeat its lab.
