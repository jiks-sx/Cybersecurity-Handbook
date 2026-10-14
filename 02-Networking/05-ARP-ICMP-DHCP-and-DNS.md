# 5. ARP, ICMP, DHCP, and DNS


## 5.1 ARP

Address Resolution Protocol (ARP) is commonly used on IPv4 local networks to discover the link-layer address associated with an IPv4 address. If a host needs to send an IPv4 packet to another device on the same Ethernet subnet, it may need that device's MAC address. If the destination is remote, it normally resolves the MAC address of the next-hop gateway instead.

ARP requests are commonly broadcast on the local link; replies provide the requested mapping. ARP does not authenticate mappings by itself, which is one reason local-network trust and monitoring matter.

Inspect the neighbor table:

```bash
ip neigh
```

Entries may be incomplete, stale, reachable, or otherwise stateful. An entry is not proof that the other device is currently healthy.

## 5.2 ICMP and ping

Internet Control Message Protocol (ICMP) carries network-layer control and error messages. `ping` commonly uses ICMP Echo Request and Echo Reply messages to test reachability and measure round-trip time.

```bash
ping -c 4 127.0.0.1
```

A failed ping does not prove a remote host is down. Firewalls may block ICMP, the return route may fail, or the target may be configured not to respond. A successful ping does not prove a TCP service or web application is healthy.

## 5.3 DHCP

Dynamic Host Configuration Protocol (DHCP) can provide IP configuration to clients. A simplified DHCPv4 process is often described as DORA:
1. **Discover:** Client looks for a DHCP server.
2. **Offer:** Server offers configuration.
3. **Request:** Client requests the offered configuration.
4. **Acknowledgement:** Server confirms the lease.

Actual exchanges can differ when a client renews a lease or already has configuration. DHCP options may include address, subnet mask, gateway, DNS servers, and lease time.

A rogue or misconfigured DHCP server can supply incorrect network settings. Enterprise networks use controls and monitoring to reduce that risk.

## 5.4 DNS

Domain Name System (DNS) stores and retrieves records associated with names. Common record types:
- `A`: IPv4 address;
- `AAAA`: IPv6 address;
- `CNAME`: alias to another DNS name;
- `MX`: mail exchange;
- `TXT`: text data often used for verification and email security;
- `NS`: authoritative name servers;
- `PTR`: reverse lookup name.

DNS is not only “turn a website into an IP.” Applications may query many record types, and resolvers may cache answers according to TTL values.

## 5.5 Recursive and authoritative DNS

A stub resolver on your device usually asks a configured recursive resolver for an answer. If the answer is not cached, the recursive resolver may query DNS hierarchy components and authoritative name servers. The authoritative server provides data for the relevant zone.

DNS over HTTPS (DoH) and DNS over TLS (DoT) can protect DNS queries between a client and resolver, but they do not make all DNS data secret everywhere, and they do not replace endpoint or resolver security.

## 5.6 Inspect DNS safely

```bash
getent hosts example.com
```

If `dig` is installed:

```bash
dig example.com A
dig example.com AAAA
dig example.com MX
```

These queries are examples against a public domain. Output depends on current DNS data, resolver configuration, caching, and network policy. `getent` follows the system's name-service configuration; `dig` queries DNS more directly.

## 5.7 Common troubleshooting logic

If a hostname fails:
1. Check whether the name is spelled correctly.
2. Check local hosts-file entries and resolver configuration.
3. Query the configured resolver.
4. Compare with another authorized resolver only if policy permits.
5. Check routing and firewall behavior.
6. Consider stale caches, split-horizon DNS, VPN requirements, or authoritative DNS problems.

Avoid assuming that changing DNS servers is always the correct fix. On corporate or VPN networks, internal names may only resolve through approved resolvers.

## 5.8 Security connection

DNS can be abused for phishing, command-and-control communication, tunneling, or data exfiltration, but unusual DNS traffic is not automatically malicious. ARP can be abused on local links. DHCP misconfiguration can redirect traffic. ICMP can carry useful diagnostics and error information. Defenders should understand normal network behavior before interpreting anomalies.

## 5.9 Summary

ARP maps IPv4 addresses to local-link addresses, ICMP supports network control and diagnostics, DHCP distributes configuration, and DNS provides distributed naming data. Each has different purposes and different failure modes.
