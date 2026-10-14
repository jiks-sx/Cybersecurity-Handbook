# Chapter 01 — Linux

> **Learn • Understand • Practice • Document**

**Difficulty:** Beginner → Intermediate  
**Estimated reading time:** 4–6 hours, excluding labs  
**Prerequisites:** Chapter 00 — Computer Fundamentals  
**Environment:** Kali Linux or another Linux virtual machine  
**Purpose:** Understand Linux as an operating system, learn the terminal properly, and build safe habits for system administration and cybersecurity.

---

## Chapter map

1. [Linux Fundamentals](01-Linux-Fundamentals.md)
2. [Terminal, Shell, and Commands](02-Terminal-Shell-and-Commands.md)
3. [Linux Filesystem](03-Linux-Filesystem.md)
4. [Users, Groups, and Permissions](04-Users-Groups-and-Permissions.md)
5. [Processes, Services, and Jobs](05-Processes-Services-and-Jobs.md)
6. [Packages, Software, and Updates](06-Packages-Software-and-Updates.md)
7. [Networking and System Inspection](07-Networking-and-System-Inspection.md)
8. [Bash Scripting Fundamentals](08-Bash-Scripting-Fundamentals.md)
9. [Safe Practical Labs](09-Safe-Practical-Labs.md)
10. [Revision, Interview Questions, and Cheat Sheet](10-Revision-Interview-and-Cheat-Sheet.md)

## How to study this chapter

- Read the explanation before copying a command.
- Run commands inside your own Linux VM or another system you are authorized to use.
- Read the output; do not treat commands as magic words.
- Record your own output, errors, and observations in your notes.
- Prefer inspection commands first. Use administrator privileges only when required and understood.
- Never paste a command from the internet into a terminal without understanding what it does.

## What you should be able to do after this chapter

- Explain what Linux is and how a Linux system is organized.
- Tell the difference between a terminal, shell, command, process, and service.
- Navigate directories and inspect files.
- Explain users, groups, ownership, and basic permission bits.
- Inspect processes, services, packages, and network configuration.
- Write and run a small Bash script safely.
- Recognize common command-line risks and troubleshoot basic problems.

## Important scope note

This chapter builds a strong foundation, not a complete reference for every Linux distribution, kernel subsystem, or security tool. Linux behavior can vary between distributions and versions. Check your system's manual pages and official documentation when details matter.

## Recommended learning method

For each topic, follow this loop:

1. **Understand** the concept.
2. **Inspect** your system with a read-only command.
3. **Predict** what the command should show.
4. **Run** it and compare the result.
5. **Explain** the output in your own words.
6. **Document** one thing that surprised you.

## Chapter completion checklist

- [ ] I can explain the Linux kernel and user space.
- [ ] I can use `pwd`, `ls`, `cd`, `cat`, `less`, `grep`, and `find`.
- [ ] I understand absolute and relative paths.
- [ ] I can explain `rwx` permissions and `sudo`.
- [ ] I can inspect processes and services without stopping them.
- [ ] I can check package and network information.
- [ ] I can run a small Bash script and explain each line.
- [ ] I completed the safe labs and wrote down my results.
