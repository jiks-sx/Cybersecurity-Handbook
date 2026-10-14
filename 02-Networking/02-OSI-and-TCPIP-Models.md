# 2. OSI and TCP/IP Models


## 2.1 Why use layers?

Networking involves many independent tasks: moving signals, addressing local devices, routing between networks, delivering data to the correct application, and defining application messages. Layered models organize these responsibilities and help people discuss where a problem may exist.

Layers are useful mental models, not physical boxes that every real protocol fits into perfectly.

## 2.2 The seven-layer OSI model

| Layer | Name | Main idea | Example concepts |
|---:|---|---|---|
| 7 | Application | Services used by applications | HTTP, DNS, SMTP |
| 6 | Presentation | Data representation, encoding, encryption concepts | Formats, serialization |
| 5 | Session | Managing communication sessions | Session concepts |
| 4 | Transport | End-to-end delivery between processes | TCP, UDP |
| 3 | Network | Logical addressing and forwarding between networks | IPv4, IPv6, routing |
| 2 | Data Link | Delivery on a local link | Ethernet, Wi-Fi MAC |
| 1 | Physical | Signals and transmission medium | Radio, copper, fiber |

Some protocols span multiple conceptual layers. TLS, for example, is often discussed between application and transport layers in traditional diagrams, but its placement depends on the model and context.

## 2.3 The TCP/IP model

A common four-layer model is:

| TCP/IP layer | Typical responsibilities | Approximate OSI mapping |
|---|---|---|
| Application | Application protocols and data formats | OSI 5–7 |
| Transport | Process-to-process delivery | OSI 4 |
| Internet | IP addressing and routing | OSI 3 |
| Link / Network access | Local-link delivery and physical transmission | OSI 1–2 |

Some references use five layers by separating the physical layer from the link layer. The models differ in purpose and terminology; do not treat one mapping as an exact universal law.

## 2.4 Encapsulation and headers

Each layer adds information needed by the next stage. A simplified HTTP-over-TCP-over-IP-over-Ethernet packet may look like:

```text
[ Ethernet header ][ IP header ][ TCP header ][ HTTP data ][ Ethernet trailer ]
```

Header fields help the receiver interpret the data. For example:
- Ethernet addresses identify local-link endpoints.
- IP addresses identify network-layer endpoints.
- TCP ports identify transport endpoints.
- HTTP defines application-level requests and responses.

The exact fields and lengths vary by protocol version and options.

## 2.5 How to use the models for troubleshooting

Think in layers, but test hypotheses rather than mechanically blaming one layer.

Example: a hostname does not open in a browser.
1. Is the device's interface up?
2. Does it have a usable IP address?
3. Is there a route to the destination?
4. Does name resolution work?
5. Can the host establish a transport connection?
6. Is TLS negotiation succeeding?
7. Is the application returning an error?

A browser error can be caused by DNS, routing, a firewall, TLS, the server, or the browser itself. Use evidence to narrow it down.

## 2.6 Encapsulation is not always one-to-one

One application message may span multiple TCP segments. A single TCP segment can carry part of an application message or several small pieces, depending on buffering and protocol behavior. IP fragmentation may occur in some circumstances, although modern systems try to avoid it in many paths. Packet captures show what was captured at a specific point—not necessarily the entire logical message.

## 2.7 Common mistakes

- Memorizing layer names without understanding their roles.
- Assuming every protocol belongs neatly to exactly one OSI layer.
- Thinking a successful ping proves a web application is healthy.
- Thinking an Ethernet MAC address is routed unchanged across the internet.
- Assuming packet capture at one interface shows every packet on the path.

## 2.8 Practice

For each item, name the most relevant layer or layers and explain why:
- Ethernet MAC address;
- IP route;
- TCP destination port;
- DNS query;
- fiber cable;
- HTTP response status;
- TLS certificate validation.

There may be more than one reasonable answer because real protocols do not always align perfectly with a teaching model.

## 2.9 Summary

The OSI and TCP/IP models organize networking responsibilities. Use them to communicate and structure troubleshooting, while remembering that real protocols can span layers and real faults require evidence.
