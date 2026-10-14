# 3. MAC, IP, IPv4, IPv6, and Subnetting


## 3.1 MAC address vs IP address

A MAC address is used by link-layer technologies such as Ethernet and Wi-Fi. An IP address is used at the network layer to identify an interface's logical network location for packet delivery.

A common analogy:
- IP address: a logical destination used to route traffic between networks;
- MAC address: a local-link destination used to deliver a frame on one network segment.

The analogy is imperfect. MAC addresses can be changed or randomized, and not every link technology uses Ethernet-style MAC addresses.

## 3.2 Does a MAC address travel across the internet?

Normally, an Ethernet frame is delivered only across one local link. When a router forwards an IP packet to another link, it creates a new link-layer frame appropriate for that next link. The source and destination MAC addresses therefore normally change at each routed Ethernet hop, while the IP source and destination generally remain the same end-to-end—subject to NAT, tunneling, proxies, and other mechanisms.

## 3.3 IPv4 basics

IPv4 addresses are 32 bits, commonly written as four decimal octets, for example `192.0.2.10`. Each octet represents eight bits and ranges from 0 to 255.

The address is interpreted together with a prefix length or subnet mask. The prefix says how many leading bits describe the network portion for that subnet.

Examples use documentation-only addresses:
- `192.0.2.10`
- `198.51.100.20`
- `203.0.113.5`

These ranges are reserved for examples and documentation, not ordinary public host assignment.

## 3.4 Prefix notation

`192.0.2.10/24` means the first 24 bits are the prefix. The corresponding subnet mask is `255.255.255.0`.

For a `/24`:
- total IPv4 addresses: \(2^{32-24}=256\);
- in a typical conventional IPv4 subnet, 254 addresses are usable by hosts because the all-zero host portion identifies the network and the all-one host portion is the broadcast address.

This familiar usable-host rule has exceptions and does not apply identically to every prefix, especially `/31` point-to-point links and `/32` host routes.

## 3.5 Calculate a simple subnet

For `192.0.2.10/24`:
- network: `192.0.2.0`;
- broadcast: `192.0.2.255`;
- conventional host range: `192.0.2.1` through `192.0.2.254`.

For `192.0.2.10/26`, each subnet contains \(2^{6}=64\) addresses. The last octet's blocks begin at 0, 64, 128, and 192. Address `.10` belongs to the `192.0.2.0/26` block, whose conventional host range is `.1` through `.62`, with `.63` as broadcast.

## 3.6 Private and special IPv4 ranges

Common private IPv4 ranges are:
- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

They are intended for private networks and are not globally routed as ordinary public destinations. Private does not mean automatically secure: a compromised device inside a private network may still attack other reachable devices.

Other special ranges include loopback (`127.0.0.0/8`), link-local (`169.254.0.0/16`), and documentation ranges. Do not assume that every non-public address has the same purpose.

## 3.7 IPv6 basics

IPv6 uses 128-bit addresses, typically written in hexadecimal groups separated by colons. A simplified example is `2001:db8::10`; `2001:db8::/32` is reserved for documentation.

IPv6 supports address shortening rules:
- leading zeros in a group can be omitted;
- one consecutive run of zero groups can be compressed with `::` in an address representation.

IPv6 does not use broadcast in the same way as IPv4; it uses multicast and other mechanisms. IPv6 security must be considered even if an administrator thinks the network is “IPv4 only,” because IPv6 may still be enabled.

## 3.8 Default gateway and routing table

A default gateway is the next hop used when no more specific route matches a destination. The routing table may contain local-subnet routes, VPN routes, container routes, and a default route.

Inspect your system:

```bash
ip address
ip route
ip -6 route
```

Do not publish your complete network configuration without reviewing it for internal addresses and host details.

## 3.9 DHCP and static configuration

DHCP can provide configuration such as an IP address, prefix/subnet mask, default gateway, DNS resolver, and lease duration. Static configuration is set manually or through management software. Both approaches require careful address planning to avoid conflicts and incorrect routes.

## 3.10 Common mistakes

- Treating a MAC address as the internet-wide address of a device.
- Assuming a private IP means a system is protected.
- Forgetting that IPv6 may be active.
- Calculating host counts without considering prefix-specific exceptions.
- Assuming the default gateway is always the same address on every network.
- Publishing local IPs and hostnames without considering privacy.

## 3.11 Practice

Run `ip address` and `ip route` on your own Linux VM. Identify:
- loopback interface;
- active network interface;
- assigned IP address and prefix;
- default route, if present;
- whether IPv6 addresses appear.

Then calculate the network, broadcast, and conventional host range for `198.51.100.77/26`.

## 3.12 Summary

MAC addresses support local-link delivery; IP addresses support logical addressing and routing. Prefixes define network boundaries, gateways provide next hops, and IPv4/IPv6 behavior must be understood separately.
