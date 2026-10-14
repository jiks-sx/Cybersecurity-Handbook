# 12. Revision and Interview Questions

## Quick revision

- A computer receives input, processes instructions, stores information, and produces output.
- Hardware is physical; software contains instructions; firmware helps start or control devices.
- The CPU runs instructions, RAM holds working data, and storage keeps files for later.
- Binary uses `0` and `1`; hexadecimal is a shorter way to write binary values.
- The operating system manages resources, and the kernel is its privileged core.
- A process is a running program; a thread is an execution path inside a process.
- System calls let programs request services from the kernel.
- Filesystem permissions help control who can read, change, or execute files.
- Firmware and boot settings are part of platform security.
- VMs and containers are useful for labs, but they still need safe configuration.

## Interview questions

Try answering in your own words before checking the chapter.

1. What is the difference between hardware, software, and firmware?
2. What does a CPU do?
3. Why does a computer need RAM if it already has an SSD?
4. What is a bit? What is a byte?
5. Why do security tools often display hexadecimal values?
6. What happens in the fetch–decode–execute model?
7. What is the difference between a program and a process?
8. What is a thread?
9. What does an operating system do?
10. What is the kernel?
11. What is a system call?
12. What is virtual memory?
13. What is the difference between stack and heap memory?
14. What do `r`, `w`, and `x` mean in Linux permissions?
15. Why should you avoid running every command as root?
16. What happens during a normal computer startup?
17. How is a VM different from a container?
18. Why might deleting a file not erase all of its old data?
19. Which commands can inspect CPU, RAM, storage, and processes?
20. What should you do before running an unfamiliar command?

## Small tasks

- [ ] Convert decimal `5` into binary.
- [ ] Convert binary `1111` into decimal and hexadecimal.
- [ ] Explain the CPU, RAM, and SSD using your own example.
- [ ] Run the Linux inspection commands and note what each one tells you.
- [ ] Explain why one program should not read every other program's private memory.
- [ ] Look at a test file's permissions with `ls -l` and explain each group.
- [ ] Draw the startup process from power-on to login.
- [ ] Explain one difference between a VM and a container.
- [ ] Write down three questions you want to explore in later chapters.

## References

- [Linux man-pages](https://www.kernel.org/doc/man-pages/)
- [Linux kernel documentation](https://docs.kernel.org/)
- [The Open Group — POSIX specifications](https://pubs.opengroup.org/onlinepubs/9799919799/)
- [UEFI specifications](https://uefi.org/specifications)
- [NIST Computer Security Resource Center](https://csrc.nist.gov/)
- [Intel Software Developer Manuals](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html)
- [Python documentation](https://docs.python.org/3/)

## Completion checklist

- [ ] I can explain the main computer parts without reading the definitions.
- [ ] I understand basic binary and hexadecimal.
- [ ] I can explain the difference between RAM and storage.
- [ ] I understand the roles of the OS and kernel.
- [ ] I can tell the difference between a program, process, and thread.
- [ ] I understand why file permissions matter.
- [ ] I completed the read-only Linux lab.
- [ ] I tried the interview questions and small tasks.
- [ ] I wrote down the topics I need to revisit.

## Scenario-based questions

1. A computer is slow. Which observations would help you decide whether CPU, RAM, storage, or a particular application is involved?
2. A file is named `invoice.pdf`, but you do not trust its source. Why is the extension not enough to establish its type?
3. A process uses a lot of CPU. What evidence would you collect before deciding it is malicious?
4. A script runs as root but can be edited by an ordinary user. What is the security problem?
5. A lab VM is intentionally vulnerable. Which virtualisation and network settings would you check before starting it?
6. A laptop is lost while powered off. How can disk encryption help, and what does it not solve?
7. A program crashes after accessing memory incorrectly. Why does a crash not automatically prove that an exploit occurred?
8. A container is running as root and has access to a sensitive host directory. Why is this risky?

## Answers: check your understanding

Try answering first, then compare with these short reminders:

1. Use observations such as `top`, `free -h`, `df -hT`, process details, and logs. A single metric is not enough.
2. Extensions can be changed. File content and format need to be inspected safely.
3. Check the command line, executable path, user, parent process, start time, logs, and expected workload.
4. The user may be able to change code that later runs with higher privileges.
5. Check network mode, exposed services, shared folders, clipboard, USB integration, and snapshots.
6. Encryption can make offline data access harder without the key, but it does not protect an unlocked session or every account.
7. Crashes have many causes. More evidence is needed to establish exploitability.
8. The container may have access to resources beyond the intended application boundary.

## Further study map

After this chapter, continue to:

- **Linux:** shell, files, permissions, processes, services, logs, and package management.
- **Networking:** IP addresses, DNS, TCP/UDP, routing, ports, and HTTP.
- **Programming:** variables, functions, data structures, error handling, and safe input handling.
- **Web security:** browser/server interaction, authentication, sessions, access control, and input validation.
- **Defensive security:** logs, baselines, patching, least privilege, and incident response.
