# 8. Network Security and Traffic Inspection

## 8.1 Network security goals

Common goals include:
- **Confidentiality:** Prevent unauthorized disclosure.
- **Integrity:** Detect or prevent unauthorized modification.
- **Availability:** Keep services usable.
- **Segmentation:** Limit unnecessary communication between systems.
- **Visibility:** Collect enough telemetry to investigate faults and threats.
- **Least privilege:** Allow only necessary access.

These goals can conflict. For example, more logging can improve visibility but may collect sensitive data that requires protection and retention controls.

## 8.2 Firewalls

A firewall applies rules to network traffic. Depending on its type, it may filter by addresses, ports, protocol, connection state, or application-level information. Host firewalls protect a single machine; network firewalls control traffic crossing a boundary.

A default-deny policy can reduce unnecessary exposure, but rules must allow legitimate required traffic. Misconfigured rules can lock administrators out or break applications.

## 8.3 Segmentation and least exposure

Place systems into appropriate network zones and allow only required paths. Guest devices generally should not have the same access as management systems. Databases often should be reachable only from application servers and administrators who need access.

Segmentation is not simply making different subnets. Routing and firewall policy must enforce the intended boundaries.

## 8.4 Packet capture and Wireshark

A packet capture records traffic visible at a selected interface and capture point. Wireshark decodes supported protocols and lets you inspect fields, conversations, and timing.

Captures can be incomplete because of:
- capture point and interface selection;
- packet loss or capture-buffer limits;
- hardware offload behavior;
- encrypted payloads;
- VLANs, tunnels, or traffic on other interfaces;
- permissions and capture filters.

A capture shows what was observed, not necessarily every packet on the network.

## 8.5 Useful display-filter examples

In Wireshark, display filters select packets already captured. Examples:

```text
dns
icmp
tcp
udp
tcp.port == 443
ip.addr == 192.0.2.10
```

These are display filters, not capture filters. The address in the example is a documentation address; replace it only with an address relevant to an authorized lab. Filter syntax can vary by protocol fields and Wireshark version.

## 8.6 Command-line inspection

```bash
ip address
ip route
ip neigh
ss -lntup
```

If `tcpdump` is installed, a basic capture may require elevated privileges and should be limited to traffic you are authorized to inspect. A narrow, time-limited capture on your own lab interface is safer than collecting all traffic indefinitely. Be mindful that captures may contain sensitive information.

## 8.7 What encryption changes

TLS can hide application payload from a passive observer, but metadata such as endpoint IPs, ports, packet sizes, timing, and some connection details may still be visible. DNS visibility depends on the resolver method and network. Encryption does not eliminate the need for endpoint security, logging, or careful access control.

## 8.8 Defensive investigation workflow

When investigating unexpected traffic:
1. Define the question and scope.
2. Record the time, host, interface, and collection method.
3. Preserve relevant evidence and avoid altering the system unnecessarily.
4. Identify endpoints, protocols, ports, timing, and traffic volume.
5. Compare with expected behavior and known application activity.
6. Correlate packet observations with host logs and process information.
7. State what is confirmed, what is only a hypothesis, and what remains unknown.
8. Protect captures and redact sensitive fields before sharing.

Do not label traffic malicious based on a port number or one unusual packet alone.

## 8.9 Common risks

- Exposed administrative services.
- Cleartext credentials on unencrypted protocols.
- Flat networks with excessive lateral access.
- Weak or missing firewall rules.
- Rogue DHCP or local address-resolution attacks.
- Outdated network-facing software.
- Sensitive data retained in packet captures or logs.

## 8.10 Summary

Network security combines access control, segmentation, encryption, patching, and visibility. Packet capture is powerful but limited and privacy-sensitive. Start with a specific question and authorized scope, then interpret evidence carefully.
