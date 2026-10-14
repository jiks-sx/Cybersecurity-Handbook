# 6. Switching, Routing, NAT, and VLANs


## 6.1 Switching

An Ethernet switch learns which source MAC addresses appear on which ports and uses its forwarding table to decide where to send frames. If the destination is unknown, the switch may flood the frame within the relevant VLAN, subject to protocol and configuration behavior.

A switch normally forwards frames within a Layer 2 domain. It does not replace IP routing between different IP networks.

## 6.2 Routing

A router forwards IP packets between networks using its routing table. It selects a route based on factors such as prefix match and routing policy. A common principle is longest-prefix match: among matching routes, the most specific prefix is generally preferred.

Inspect local routes:

```bash
ip route
ip -6 route
```

The output may contain a default route, connected routes, VPN routes, and routes for virtual interfaces. Route selection can involve policy rules and multiple tables, so the simple output is not always the whole story.

## 6.3 Default gateway

When a destination does not match a more specific route, the system may use a default route. The default gateway is a next hop, not necessarily the final destination. If the default route is missing or points to an unreachable gateway, remote connectivity may fail while local-subnet communication still works.

## 6.4 NAT

Network Address Translation (NAT) changes address information in packets. Home routers commonly use IPv4 source NAT or port address translation so several private devices can share a public IPv4 address.

NAT is not the same thing as a firewall. NAT may make unsolicited inbound connectivity less straightforward in common home configurations, but security depends on filtering rules, service exposure, IPv6 configuration, and other controls.

## 6.5 VLANs

A Virtual LAN (VLAN) separates a switched network into logical Layer 2 broadcast domains. VLANs can support segmentation, management separation, and operational organization.

VLANs do not automatically provide full security isolation. Routing between VLANs, switch configuration, trunk ports, and firewall rules determine what traffic can cross the boundaries.

## 6.6 Routing between VLANs

Devices in different IP subnets usually need a router or Layer 3 switch to communicate. Policies can restrict which VLANs are allowed to talk to each other. A common secure design separates user devices, servers, guest Wi-Fi, and management interfaces, then permits only necessary traffic.

## 6.7 Packet path example

Imagine a laptop sends traffic to a remote server:
1. The laptop checks its route table.
2. If the destination is remote, it selects the default gateway or another route.
3. It sends a local-link frame addressed to the gateway's MAC address.
4. The router receives the frame, processes the IP packet, selects an outgoing route, and forwards it.
5. A new link-layer frame is created for the next link.
6. This repeats across routers until the destination network is reached.

NAT, VPNs, tunnels, firewalls, and load balancers may change or add steps.

## 6.8 Security and troubleshooting

- Verify the correct subnet and gateway.
- Check that VLAN access and trunk configuration matches the intended design.
- Restrict management interfaces to trusted networks.
- Do not treat NAT as a substitute for access-control policy.
- Avoid exposing internal routing and device details in public screenshots.
- Change network configuration only when you understand the path to recovery; a wrong route can disconnect remote access.

## 6.9 Practice

Use `ip route` to identify your default route if one exists. Draw a diagram showing your Linux VM, its virtual network, the host, and the router. Your VM's path depends on whether the hypervisor uses NAT, bridged, host-only, or another mode.

## 6.10 Summary

Switches forward frames within Layer 2 networks; routers forward IP packets between networks. NAT rewrites address information, and VLANs create logical Layer 2 separation. These features must be configured together to create a secure network.
