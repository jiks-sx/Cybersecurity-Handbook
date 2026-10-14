# 2. Computer Components

## CPU: running instructions

CPU stands for **Central Processing Unit**. It runs instructions from programs.

Important parts include:

- **Control unit:** coordinates instruction execution.
- **ALU:** performs arithmetic and logical operations, such as adding numbers or comparing values.
- **Registers:** very small, fast places inside the CPU that hold values needed immediately.
- **Cache:** keeps copies of useful data close to the CPU so it may not need to wait for RAM as often.
- **Cores:** allow the CPU to work on multiple streams of instructions, depending on the hardware and workload.

Modern CPUs overlap work and use other techniques to improve speed, so the real process is more complicated than a simple calculator. The basic idea is still that the CPU runs machine instructions.

**Security connection:** CPU knowledge helps later with debugging, reverse engineering, and understanding some software vulnerabilities.

## RAM: working space

RAM stands for **Random Access Memory**. Think of it like a desk: the information you need while working stays on the desk, and you put it away when finished.

RAM holds data and instructions used by the operating system and running programs. It is usually volatile, meaning its contents are not kept when power is removed.

If RAM becomes full, the operating system may move some memory pages to storage. This can help keep programs running, but storage is much slower than RAM.

**Security connection:** running programs can keep sensitive information in memory, including login tokens or decrypted data. Memory dumps must be handled carefully.

## Storage: keeping files

Storage keeps information after shutdown.

- **HDD:** stores data on magnetic spinning disks and has moving parts.
- **SSD:** stores data in flash memory and has no spinning disk.
- **NVMe:** a protocol commonly used by modern SSDs over PCI Express. It is not a type of flash memory itself.

Storage can contain the operating system, programs, documents, logs, and virtual-machine files.

Deleting a file does not always mean its old data has been securely erased. The result depends on the filesystem, device, encryption, and other details.

**Security connection:** storage matters in digital forensics, disk encryption, backups, log review, and secure disposal.

## Motherboard, buses, and I/O

The motherboard connects the CPU, RAM, storage interfaces, and other devices. Components communicate through connections such as memory channels, PCI Express, and USB.

Input/output (I/O) devices let the computer exchange information with the outside world. A keyboard sends input; a screen displays output; a network adapter can both send and receive data.

The operating system often uses a **device driver** to communicate with hardware. Drivers can have powerful access, so their security matters.

## Quick check

1. How are RAM and an SSD different?
2. What does the CPU do?
3. Why can a vulnerable device driver be a security concern?

## Behind the scenes: reading a file

When an application reads a file, a simplified path is:

1. The application requests the file through an operating-system interface.
2. The kernel checks the request and the caller's permissions.
3. The filesystem finds the file's metadata and the locations of its data.
4. The storage device supplies data, unless useful data is already in a cache.
5. The operating system makes the data available to the application.

The exact path depends on the operating system, filesystem, cache state, and device. Applications do not usually control the physical SSD directly.

## CPU performance is not only clock speed

CPU performance can depend on architecture, core count, cache, memory speed, power limits, cooling, and the type of task. A higher advertised clock speed alone does not prove that one CPU will be faster for every workload.

## What can go wrong?

- **Overheating:** the system may reduce performance to protect hardware.
- **Storage failure:** files may become unavailable or corrupted.
- **Driver problems:** a device may stop working correctly or cause instability.
- **Untrusted peripherals:** some devices can present unexpected interfaces or exploit vulnerabilities.
- **Physical access:** an attacker with physical access may attempt to boot external media or remove storage.

## Safe observation commands

```bash
lscpu
free -h
lsblk -f
df -hT
```

These commands show different parts of the system. They do not prove that hardware is healthy, but they give useful starting information.

## Real-world security scenario

An incident responder finds that a server is using nearly all of its memory. They should not immediately assume an attack. They can compare current resource use with a known baseline, inspect processes, check service logs, and look for recent changes. A denial-of-service attack is one possibility, but a memory leak or normal workload growth may also explain the evidence.
