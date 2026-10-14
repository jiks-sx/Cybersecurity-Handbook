# 4. TCP, UDP, Ports, and Sockets


## 4.1 Why transport protocols exist

IP moves packets between network-layer addresses, but applications also need a way to identify which process should receive data. Transport protocols provide process-to-process communication using port numbers and other mechanisms.

## 4.2 Ports

TCP and UDP port numbers are 16-bit values, from 0 to 65535. Common categories are:
- system/well-known ports: 0–1023;
- registered ports: 1024–49151;
- dynamic/private ports: 49152–65535.

These ranges describe common IANA conventions, but operating-system allocation and application behavior can vary. A port number alone does not prove which software is running or whether it is vulnerable.

## 4.3 TCP

TCP provides a reliable, ordered byte stream. It uses sequence numbers, acknowledgements, retransmission, flow control, and congestion control. Applications do not normally see TCP as a set of independent messages; they see a stream of bytes.

A simplified TCP connection establishment uses a three-way handshake:

```text
Client  ---- SYN ---->  Server
Client  <-- SYN-ACK --  Server
Client  ---- ACK ---->  Server
```

This establishes connection state, but it does not authenticate the application user or encrypt the data. TLS or application-level security may be needed for those goals.

## 4.4 UDP

UDP provides datagrams without TCP's built-in connection establishment, ordered delivery, or retransmission guarantees. Applications may build reliability or security on top of UDP when needed. DNS, voice/video traffic, and some real-time or modern transport systems use UDP in different ways.

“UDP is faster” is an oversimplification. Performance depends on application behavior, network conditions, packet size, retransmission strategy, and other factors.

## 4.5 Sockets and connection identity

A socket is an operating-system abstraction for communication. A TCP connection is commonly identified by the combination of source IP, source port, destination IP, and destination port, along with protocol context. This is often called a 4-tuple.

A server can listen on one port while handling many client connections, because each connection has a different peer tuple.

## 4.6 Listening ports

Inspect sockets on your own machine:

```bash
ss -lnt
ss -lun
ss -lntup
```

- `-l`: listening sockets;
- `-n`: numeric addresses and ports;
- `-t`: TCP;
- `-u`: UDP;
- `-p`: process information where permitted.

A port being open or listening is not automatically a security issue. Check what service is bound, which interfaces it listens on, whether authentication is enabled, whether the service is current, and who can reach it.

## 4.7 Common service ports

| Port | Common association | Important note |
|---:|---|---|
| 22/TCP | SSH | Secure remote administration when configured well |
| 53/TCP, UDP | DNS | Both transports are used in different situations |
| 67/68 UDP | DHCPv4 | Server/client roles use different ports |
| 80/TCP | HTTP | Plain HTTP does not provide TLS protection |
| 123/UDP | NTP | Time synchronization |
| 443/TCP | HTTPS over TCP | HTTP protected with TLS in common deployments |
| 443/UDP | QUIC, commonly used by HTTP/3 | Not the same as TCP-based HTTPS |
| 25/TCP | SMTP | Mail transfer; server policy varies |
| 3306/TCP | MySQL default | Defaults can be changed; exposure should be restricted |

Ports are conventions, not guarantees. A service can use a non-default port, and unrelated software can bind to a familiar port.

## 4.8 Security connection

Firewalls often match protocol, addresses, ports, connection state, and sometimes application details. Restrict unnecessary listeners, bind services to the intended interfaces, keep software updated, and use secure authentication and encryption where appropriate.

## 4.9 Mini exercise

Run `ss -lntup`. For each result you can identify, record:
- protocol;
- local address and port;
- process name, if shown;
- whether it is bound to loopback, a specific interface, or all interfaces;
- whether you expected it to be listening.

Do not terminate or reconfigure services during this exercise.

## 4.10 Summary

TCP provides a reliable ordered byte stream; UDP provides datagrams with fewer built-in delivery guarantees. Ports help deliver traffic to applications, and sockets represent communication endpoints. Always assess the service and its exposure, not just the port number.
