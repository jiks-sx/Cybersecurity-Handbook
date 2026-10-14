# 6. Operating Systems

An **operating system (OS)** manages the computer's resources and gives applications a way to use them. Examples include Windows, Linux, and macOS.

An OS helps with:

- Running programs and sharing CPU time.
- Managing memory.
- Reading and writing files.
- Using devices.
- Communicating over networks.
- Applying access permissions.

## What is the kernel?

The **kernel** is the core part of an operating system. It has special privileges so it can manage important resources.

Applications usually run in **user space**, where they have fewer privileges.

```text
+----------------------------------+
| User space                       |
| Browser | Terminal | Python      |
+----------------------------------+
             |
        System calls
             |
+----------------------------------+
| Kernel                           |
| Memory | Processes | Files       |
| Network | Device drivers         |
+----------------------------------+
             |
+----------------------------------+
| CPU | RAM | Storage | Devices    |
+----------------------------------+
```

A program normally cannot directly control every hardware resource. It requests services through approved operating-system interfaces.

## What is a system call?

A **system call** lets a program request a service from the kernel. Linux system calls are used for operations such as opening files, reading and writing data, creating processes, and using network sockets.

Libraries often provide convenient functions that use system calls behind the scenes.

## Why this matters in cybersecurity

If an ordinary application has a bug, its permissions may limit the damage. A serious kernel vulnerability can affect a much larger part of the system because the kernel has powerful access.

This is one reason operating-system updates, trusted drivers, and limited administrator access matter.

## Quick check

1. What is the job of an operating system?
2. How are user space and kernel space different?
3. What is a system call?

## Behind the scenes: reading a file through a system call

A program may call a library function to open a file. The library uses an operating-system interface, and the kernel checks the request. The kernel and filesystem resolve the path and apply access controls. If access is allowed, the program receives a file descriptor that it can use for later operations.

This is why file permissions are enforced by the operating system rather than by trusting the application to behave.

## User mode and kernel mode

Processors provide privilege levels or modes. User-space code runs with restricted privileges. Kernel code runs with elevated privileges needed to manage system resources.

When a program requests a system service, the CPU performs a controlled transition to privileged code. The kernel validates the request, performs or rejects the operation, and returns a result.

A system call is not the same as a normal function call: it crosses a protection boundary and has specific rules and costs.

## Linux commands for observation

```bash
uname -a
cat /etc/os-release
dmesg --level=err,warn
```

The ability to read kernel logs can depend on distribution policy and user privileges. Do not change security settings just to make a command work.

## Real-world security scenario: privilege escalation

A privilege-escalation bug allows a user or process to gain more access than intended. Possible causes include a kernel vulnerability, unsafe privileged service, weak file permissions, or an overly powerful configuration.

A proper investigation identifies the exact boundary that failed and checks whether a patch or configuration fix exists. Do not test escalation techniques on a system unless you own it or have explicit authorisation.

## Common misunderstanding

The kernel is not the whole operating system. User-space services, libraries, tools, and applications are also important parts of the operating environment.
