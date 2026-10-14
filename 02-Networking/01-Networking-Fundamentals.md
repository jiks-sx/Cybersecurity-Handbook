# 1. Networking Fundamentals


## 1.1 What is a computer network?

A computer network is a group of devices connected so they can exchange data. Devices may include laptops, phones, servers, printers, routers, switches, cameras, and virtual machines.

The connection can use copper cable, fiber, Wi-Fi, or another medium. A network also needs rules and addressing so devices know how to format, send, receive, and interpret information.

## 1.2 Why networks exist

Networks allow devices to:
- access websites and online applications;
- share files and printers;
- communicate with services hosted elsewhere;
- send messages and stream media;
- manage devices centrally;
- exchange telemetry, logs, and business data.

The same connectivity can create security risks. An exposed service, weak authentication, or poorly separated network may let an attacker reach resources that should not be accessible.

## 1.3 Common network devices

- **End device/host:** A laptop, phone, server, or other device that sends or receives data.
- **Switch:** Commonly forwards Ethernet frames within a local network using MAC-address information.
- **Router:** Forwards IP packets between networks using routing information.
- **Wireless access point (AP):** Connects wireless clients to a network.
- **Firewall:** Enforces traffic rules based on its capabilities and configuration.
- **Modem/ONT:** Connects a local network to an internet access service, depending on the technology.
- **Server:** A system that provides a service to clients; this is a role, not a special physical shape of computer.

A home router may combine routing, switching, Wi-Fi, NAT, and firewall functions in one box.

## 1.4 Network types

- **PAN:** Personal-area network, such as a short-range connection between personal devices.
- **LAN:** Local-area network, such as a home, lab, or office network.
- **WLAN:** A LAN using wireless networking.
- **WAN:** A wide-area network spanning larger geographic areas.
- **VPN:** A virtual private network creates a logical protected connection across an underlying network. VPNs vary in design and do not automatically make every activity anonymous or safe.

The labels describe scope or function, but real network boundaries can be more complex.

## 1.5 Topology

Topology describes how network components are connected, physically or logically.

- **Star:** Devices connect to a central switch or access point.
- **Bus:** Devices share a common communication medium; classic examples are mostly historical.
- **Ring:** Connections form a ring.
- **Mesh:** Nodes have multiple interconnections, which can improve alternate paths but increase complexity.
- **Hybrid:** A combination of structures.

A physical diagram and a logical traffic-flow diagram may look different. When troubleshooting, record both when the distinction matters.

## 1.6 Packets, frames, and segments

Networking breaks application data into units that can be transmitted and processed. Names depend on the protocol layer:

- **Application data:** The information an application wants to send.
- **TCP segment:** TCP header plus TCP data.
- **UDP datagram:** UDP header plus UDP data.
- **IP packet:** IP header plus payload, often a TCP segment or UDP datagram.
- **Ethernet frame:** Ethernet header and trailer around an IP packet on a typical Ethernet link.

These are simplified examples. Not every network uses Ethernet, and other protocols may have different structures.

## 1.7 Encapsulation: a simplified example

Suppose a browser requests a webpage:

```text
HTTP request data
        ↓
TCP adds source/destination ports and control information
        ↓
IP adds source/destination IP addresses
        ↓
Ethernet adds local-link source/destination MAC addresses
        ↓
Bits/signals travel over the medium
```

At the receiving end, the layers process the data in reverse order. This is called decapsulation.

When a router forwards traffic, it normally removes the incoming link-layer framing and creates new link-layer framing for the next link. The IP packet is forwarded according to routing rules, although some fields may change along the way.

## 1.8 What happens when you open a website?

A simplified sequence:
1. The device needs to resolve the website's hostname, often using DNS.
2. It determines whether the destination is local or needs a router, based on its IP configuration and routing table.
3. It resolves the next-hop link-layer address if needed, for example through ARP on IPv4 Ethernet.
4. It sends packets toward the next hop.
5. Routers forward packets between networks.
6. The destination server receives the traffic and processes the transport and application protocols.
7. The server sends a response back through the network.
8. The browser interprets the response and renders the page.

Modern web connections can reuse DNS results, connections, TLS sessions, caches, proxies, CDNs, and HTTP/2 or HTTP/3. Therefore, the actual sequence may differ from this introductory model.

## 1.9 Bandwidth, throughput, latency, and loss

- **Bandwidth/capacity:** The nominal or available data-carrying rate of a link, often measured in bits per second.
- **Throughput:** The rate of successful data transfer observed over a period.
- **Latency:** The time it takes for data to travel between points; measurements depend on the method and direction.
- **Jitter:** Variation in packet delay.
- **Packet loss:** Packets that fail to reach the intended destination or are discarded along the path.

A link advertised at 1 Gbit/s does not guarantee an application will achieve 1 Gbit/s. Protocol overhead, congestion, Wi-Fi conditions, server limits, and many other factors matter.

## 1.10 Security connection

Network segmentation can limit which systems can communicate. Firewalls can restrict traffic. Encryption can protect data in transit. Monitoring can help identify unusual behavior. None is a complete solution by itself: rules can be misconfigured, endpoints can be compromised, and encrypted traffic can still reveal metadata.

## 1.11 Mini exercise

On your own system, draw a simple diagram containing your laptop, home router/access point, internet connection, and one remote service. Mark:
- where the local network ends;
- where a router forwards traffic;
- which devices you control;
- which parts you cannot directly administer.

## 1.12 Summary

Networking connects devices through shared media and protocol rules. Understanding devices, packets, frames, addressing, and performance measurements creates the foundation for troubleshooting and security analysis.
