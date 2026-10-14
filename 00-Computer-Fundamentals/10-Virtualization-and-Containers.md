# 10. Virtualization and Containers

## Virtual machines

A **virtual machine (VM)** acts like a separate computer created with virtual hardware. It can run its own guest operating system.

For example, a person might run Kali Linux inside VMware on a Windows laptop. Windows is the host operating system, and Kali is the guest operating system.

```text
+--------------------+  +--------------------+
| Kali Linux VM      |  | Another guest VM   |
| Tools and programs |  | Tools and programs |
+--------------------+  +--------------------+
|          Virtualization software             |
+----------------------------------------------+
| Host OS and physical hardware                |
+----------------------------------------------+
```

VMs are useful for cybersecurity labs because snapshots can help restore a previous state.

A VM is not a perfect security wall. Shared folders, clipboard sharing, USB passthrough, network settings, and hypervisor vulnerabilities can create risks.

## Containers

Containers isolate applications using operating-system features. Linux containers commonly use namespaces and control groups. Unlike a typical VM, a container usually shares the host kernel.

| Virtual machine | Container |
|---|---|
| Usually runs its own guest kernel | Usually shares the host kernel |
| Virtualises a computer platform | Isolates an application environment |
| Often uses more resources | Often has a smaller footprint |
| Can run a compatible guest OS | Depends on the host kernel model |

Containers are useful, but they are not automatically secure. Excessive privileges or access to sensitive host files can weaken isolation.

## Safer lab habits

- Practise only on systems you own or have permission to test.
- Keep intentionally vulnerable machines isolated.
- Use controlled NAT or host-only networking when suitable.
- Take a snapshot before risky experiments.
- Avoid putting personal files or real credentials in a test VM.
- Keep your host and virtualisation software updated.

## Quick check

1. What is a host OS? What is a guest OS?
2. What is one key difference between a VM and a container?
3. Name two habits that make a practice lab safer.

## Behind the scenes: what a VM needs

A hypervisor provides virtual hardware to a guest operating system. The guest sees virtual CPUs, memory, disks, and network adapters. The hypervisor maps those resources to the host's physical resources.

VM snapshots can save a state that is useful for recovery. A snapshot is not the same as an independent backup: it may depend on the original virtual disk and snapshot chain.

## Networking modes

- **NAT:** the guest commonly shares the host's network path through address translation. Inbound reachability depends on configuration.
- **Bridged:** the guest appears more like a separate device on the local network.
- **Host-only / private network:** limits connectivity to a defined lab network, depending on the hypervisor setup.

Names and exact behaviour vary by virtualisation product. Always check the current network configuration before starting a vulnerable machine.

## Containers in more detail

Namespaces can provide separate views of resources such as process IDs, mounts, and network interfaces. Control groups (cgroups) can limit and account for resource use. Container images package an application and its dependencies, but images may contain vulnerable software or secrets if built carelessly.

## Real-world scenario: vulnerable lab machine

Before starting an intentionally vulnerable VM:

1. Confirm it is a lab image you are allowed to use.
2. Choose an isolated or carefully controlled network.
3. Disable shared folders and clipboard integration if not needed.
4. Take a snapshot or backup.
5. Keep real credentials and personal files out of the guest.
6. Shut down the lab when finished.

## Common mistakes

- Assuming NAT means a guest can never be reached from elsewhere.
- Assuming a snapshot is a complete backup.
- Assuming a container automatically provides a strong boundary.
- Leaving vulnerable services exposed to a public network.
