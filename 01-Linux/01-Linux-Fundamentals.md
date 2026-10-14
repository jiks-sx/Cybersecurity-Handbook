# 1. Linux Fundamentals

** Beginner **   

## 1.1 What is Linux?

Linux is an operating-system kernel. The kernel is the central part of an operating system that manages hardware and provides controlled access to resources such as CPU time, memory, storage, and devices.

In everyday conversation, people often say “Linux” to mean a complete operating system built around the Linux kernel. A complete system usually includes the kernel, command-line tools, libraries, system services, a package manager, and sometimes a graphical desktop.

Examples of Linux distributions include Kali Linux, Ubuntu, Debian, Fedora, and Linux Mint. They share the Linux kernel family but can differ in default software, release schedules, configuration, and package management.

## 1.2 Why does Linux matter in cybersecurity?

Linux is used in servers, cloud infrastructure, network appliances, embedded devices, developer workstations, containers, and security testing environments. Many security tools run on Linux, but the important skill is understanding the operating system—not memorizing a tool list.

A security learner may use Linux to:
- inspect files and permissions;
- understand network connections;
- read logs and investigate processes;
- automate repetitive tasks;
- run authorized labs and security tools;
- administer and harden systems.

## 1.3 The main parts of a Linux system

A simplified view:

```text
User
 |
 v
Applications and command-line tools
 |
 v
Libraries and system services
 |
 v
System calls (controlled requests to the kernel)
 |
 v
Linux kernel
 |       |        |
CPU    Memory   Devices / Storage / Network
```

This is a learning model, not a strict diagram of every system. Some services communicate over sockets, applications can use different libraries, and the kernel has many subsystems.

### Kernel

The kernel runs with high privilege. It schedules tasks, manages virtual memory, handles system calls, coordinates device drivers, and enforces important boundaries between processes.

### User space

User space is where most applications and ordinary services run. They generally cannot directly perform every privileged operation. Instead, they request kernel services through defined interfaces.

### Shell and command-line tools

A shell reads commands and launches programs. Utilities such as `ls`, `cp`, `grep`, and `ip` are separate programs or shell built-ins; they are not all part of the kernel.

### System services

Services are background programs that provide functions such as logging, time synchronization, networking, or remote access. The exact service manager and service names depend on the distribution.

## 1.4 What happens when you run a command?

Suppose you type:

```bash
ls -l /etc
```

At a high level:
1. The terminal sends your typed text to the shell.
2. The shell parses the command and its arguments.
3. The shell finds the `ls` program using its command-search rules.
4. The program starts as a process.
5. It asks the operating system for directory and file metadata.
6. The kernel checks access permissions and returns allowed information.
7. `ls` formats the result and writes it to standard output.
8. The terminal displays the output.

This is simplified: shells may optimize some commands, use built-ins, or behave differently depending on aliases and configuration.

## 1.5 What does “open source” mean?

Open source means the source code is available under a license that allows specified forms of use, study, modification, and redistribution. It does not mean every program is automatically secure. Security still depends on code quality, configuration, maintenance, dependencies, and how the system is operated.

## 1.6 Check your Linux environment

Run these commands individually:

```bash
uname -a
cat /etc/os-release
whoami
id
pwd
```

What they tell you:
- `uname -a`: kernel and system information.
- `cat /etc/os-release`: distribution identity and version details, when provided.
- `whoami`: effective username shown for the current process.
- `id`: user and group identity information.
- `pwd`: current working directory.

The output varies by system. Do not publish full system details if they expose hostnames, usernames, internal addresses, or other information you want to keep private.

## 1.7 Linux is not the same as Kali Linux

Kali is a Linux distribution designed for penetration testing, security auditing, forensics, and related work. It includes many security tools, but it is not a shortcut to understanding security. For daily work, a general-purpose distribution may be more convenient; for a security lab, Kali can provide a ready-made toolkit.

Use security tools only on systems you own or have clear permission to test.

## 1.8 Common misunderstandings

- **“Linux is a single operating system.”** In practice, many distributions package the Linux kernel with different tools and defaults.
- **“The terminal is Linux.”** The terminal is an interface; the shell interprets commands; Linux is the kernel at the center of the operating system.
- **“Open source means bug-free.”** Public source code can still contain vulnerabilities.
- **“Root is just another username.”** Root normally has extensive privileges, so mistakes made as root can have system-wide effects.
- **“Every Linux command works everywhere.”** Commands, options, and paths can differ by distribution and software version.

## 1.9 Quick practice

Write down the output and answer:
1. Which distribution and version are you using?
2. Which kernel version is running?
3. What username and groups does your account use?
4. What directory are you currently in?
5. Which of these commands only inspect information, and which could change system state?

## 1.10 Summary

Linux is the kernel at the core of many operating systems. A working Linux system adds user-space tools, libraries, services, and often a desktop environment. Understanding the boundary between applications and the kernel helps explain permissions, processes, files, networking, and many security issues.
