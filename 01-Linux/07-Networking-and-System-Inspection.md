# 7. Networking and System Inspection


## 7.1 Start with observation

A Linux machine can have loopback, Ethernet, Wi-Fi, VPN, virtual-machine, and container interfaces. Interface names vary by distribution and hardware.

```bash
ip address
ip route
ip link
```

- `ip address` shows interface addresses.
- `ip route` shows routing information.
- `ip link` shows link-layer interface state.

An address beginning with `127.` is IPv4 loopback; `::1` is IPv6 loopback. Loopback traffic stays within the local system.

## 7.2 DNS and name resolution

DNS helps map domain names to records such as IP addresses. Linux name resolution may involve `/etc/hosts`, resolver configuration, caching services, and DNS servers.

```bash
cat /etc/hosts
cat /etc/resolv.conf
getent hosts example.com
```

`/etc/resolv.conf` may be generated or managed by another service, so manually editing it may not be persistent. Use a domain you are allowed to query and follow your network's rules.

## 7.3 Test connectivity carefully

```bash
ip route
ping -c 4 127.0.0.1
```

The loopback ping checks local IP networking, not whether the internet is available. A failed ping to a remote host does not prove the host is down; ICMP may be filtered or routing may be restricted.

For name resolution, `getent hosts example.com` can test the system's configured lookup path. Avoid repeatedly sending traffic to systems without authorization.

## 7.4 Listening sockets

A listening socket is an endpoint waiting for connections. It may belong to a web server, SSH service, local development tool, or another application.

```bash
ss -lntup
```

This asks `ss` to display listening TCP/UDP sockets with numeric addresses and process details where permissions allow. Some process information may require elevated rights. Do not expose the full output publicly without checking for sensitive host and service details.

A listening port is not automatically a vulnerability. Risk depends on the service, version, configuration, authentication, network exposure, and surrounding controls.

## 7.5 Hostname, time, and system details

```bash
hostname
date
timedatectl status
uname -r
```

Time matters for logs, certificates, authentication, and incident timelines. If system time is incorrect, investigate the configured time service rather than making random changes.

## 7.6 Basic troubleshooting order

When a network application fails, work from local to remote:
1. Is the interface present and up?
2. Does it have the expected address?
3. Is there a suitable route?
4. Does name resolution work?
5. Can the destination be reached using the relevant protocol?
6. Is the service listening and configured correctly?
7. Are firewall rules, proxy settings, VPNs, or access policies involved?

Do not assume that one failed command identifies the root cause. Record each test and what it proves—and what it does not prove.

## 7.7 VM networking note

Virtual machines may use NAT, bridged, host-only, or other network modes. NAT often lets the guest initiate outbound connections through the host's network path, while host-only networking is commonly used for isolated guest-to-host or guest-to-guest labs. Exact behavior depends on the hypervisor configuration.

Before running vulnerable lab machines, verify the VM network mode and keep intentionally vulnerable systems away from networks where they do not belong.

## 7.8 Summary

Use `ip`, `getent`, `ss`, and system-inspection commands to build evidence in a logical order. A network observation is a clue, not a full diagnosis. Understand VM networking before exposing lab machines.
