# 5. Memory Management

## A simple memory hierarchy

```text
CPU registers
      |
CPU cache
      |
RAM
      |
SSD or HDD
```

This is a simplified picture. Registers and caches are small and fast. RAM is larger. Storage can hold much more data and keeps it after shutdown.

## Virtual memory

Virtual memory gives each process its own virtual address space. The operating system and CPU work together to map virtual addresses to physical memory and apply access rules.

This helps keep programs separate. A normal application should not be able to read or change another application's private memory just because it wants to.

Virtual memory is not another name for RAM. Some memory pages can be backed by files or storage, but storage is much slower than physical RAM.

## Stack and heap

Programs commonly use memory areas called the **stack** and **heap**.

- **Stack:** often holds information for active function calls and local variables.
- **Heap:** commonly holds data created dynamically while a program runs.

These are software memory-management concepts, not separate physical memory chips.

## Memory bugs and security

If software handles memory incorrectly, it can cause bugs such as:

- **Buffer overflow:** data goes beyond the space intended for it.
- **Use-after-free:** software tries to use memory after it has been released.
- **Out-of-bounds access:** software reads or writes outside an allowed range.

Such bugs can crash a program and, in some cases, create security risks.

Defences include careful bounds checking, memory-safe languages, address-space layout randomisation (ASLR), and non-executable memory. Each defence has limits, so good design and testing still matter.

## Quick check

1. Why does each process have a virtual address space?
2. What is one difference between stack and heap?
3. Name one example of a memory-safety bug.

## Behind the scenes: memory address translation

A program uses virtual addresses. The CPU's memory-management unit (MMU), using page-table information maintained by the operating system, translates those addresses to physical memory locations. A translation lookaside buffer (TLB) caches recent translations to speed up this work.

The OS can set permissions on memory pages, such as whether they may be read, written, or executed. These rules help isolate processes and prevent some unsafe operations.

## What is paging?

Memory is commonly managed in fixed-size units called pages. When a program accesses a page that is not currently mapped as expected, a page fault occurs. A page fault is not automatically an error: the operating system may need to load a page, create a mapping, or handle an invalid access.

If memory pressure causes pages to be moved to storage, performance can fall because storage is much slower than RAM.

## Real-world security scenario: a process crashes

A crash may be caused by invalid memory access, a software bug, resource exhaustion, or another problem. A useful investigation starts with the error message, logs, version information, and a reproducible test. Do not assume every crash is an exploit.

## Memory protections

Common protections include:

- **ASLR:** randomises important memory locations, making some attacks less predictable.
- **DEP/NX:** helps prevent data pages from being executed as code.
- **Stack canaries:** can detect some forms of stack corruption.
- **Memory-safe languages:** remove or reduce many classes of memory-safety errors.
- **Least privilege and process isolation:** limit what a compromised process can access.

These controls reduce risk but do not replace secure design, updates, and testing.

## Safe learning exercise

Use a debugger only on your own test program or an authorised lab. Start by observing variables and call stacks in a tiny program. Do not attach debuggers to other users' processes or production systems without permission.
