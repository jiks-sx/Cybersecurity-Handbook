# 10. Revision, Interview Questions, and Cheat Sheet

## 10.1 Quick revision

- Linux is the kernel; a distribution combines the kernel with tools, libraries, services, and package choices.
- A terminal displays input/output; a shell parses commands; a process is a running program.
- `/` is the filesystem root; `/root` is commonly the root user's home directory.
- Absolute paths start at `/`; relative paths are interpreted from the current working directory.
- Standard streams are stdin (0), stdout (1), and stderr (2).
- Pipes pass output from one command to another.
- UIDs and GIDs identify users and groups.
- `rwx` permission bits mean different things for regular files and directories.
- `sudo` provides authorized elevated execution; it should be used deliberately.
- Processes, services, and shell jobs are related but different concepts.
- Package managers install and track software; repository trust matters.
- Network commands show evidence, not necessarily the complete cause of a problem.
- Bash scripts automate tasks and must validate input and handle errors.

## 10.2 Command cheat sheet

| Goal | Command |
|---|---|
| Current directory | `pwd` |
| List files, including hidden entries | `ls -la` |
| Change directory | `cd /path` |
| Show distribution details | `cat /etc/os-release` |
| Show kernel details | `uname -r` |
| Show user and groups | `id` |
| Read a text file | `less file.txt` |
| Show file metadata | `stat file.txt` |
| Search text | `grep -n 'pattern' file.txt` |
| Find files by name | `find ~/linux-practice -name '*.txt'` |
| Copy a file | `cp source destination` |
| Move/rename a file | `mv source destination` |
| Inspect permissions | `ls -l file.txt` |
| Inspect path permissions | `namei -l /path/to/file` |
| Show processes | `ps aux` |
| Show live resource use | `top` |
| Show memory summary | `free -h` |
| Show disk space | `df -h` |
| Show interfaces | `ip address` |
| Show routes | `ip route` |
| Show listening sockets | `ss -lntup` |
| Search manual | `man command` |
| Check command location | `command -v command` |

**Reminder:** Check `man command` or `command --help` for exact syntax on your system. Some commands and options differ across distributions.

## 10.3 Interview questions

### Beginner

1. What is Linux?
2. What is the difference between a kernel and a distribution?
3. What is the difference between a terminal and a shell?
4. What does `pwd` do?
5. What is the difference between an absolute and relative path?
6. What is the purpose of `/etc` and `/var`?
7. What do `stdin`, `stdout`, and `stderr` mean?
8. What is a pipe in a shell?
9. What is a process ID?
10. Why should you avoid using `sudo` for every command?

### Intermediate

11. How do file permissions work?
12. Why does execute permission mean something different on a directory?
13. What is the difference between a process and a service?
14. What is the difference between `apt update` and upgrading installed packages?
15. How would you investigate a service that does not start?
16. How would you begin troubleshooting a DNS issue?
17. Why is a listening port not automatically a vulnerability?
18. What risks can come from a script that is writable by an untrusted user?
19. Why should shell variables often be quoted?
20. What information should you redact before sharing terminal output?

## 10.4 Short answer key

1. Linux is the kernel used by many operating systems.
2. A distribution packages the kernel with user-space software, configuration, and update mechanisms.
3. The terminal is the text interface; the shell interprets commands.
4. It prints the current working directory.
5. Absolute paths start at `/`; relative paths use the current directory as their starting point.
6. `/etc` commonly contains system configuration; `/var` commonly stores changing data such as logs.
7. They are standard input, normal output, and error output.
8. A pipe connects one program's standard output to another's standard input.
9. A PID identifies a process while it exists.
10. Elevated privileges increase the impact of mistakes.
11. Basic permissions control read, write, and execute/search access for owner, group, and others.
12. Directory execute permission generally controls traversal/search.
13. A process is a running program; a service is a background program or managed function, often started and monitored by a service manager.
14. `apt update` refreshes repository metadata; upgrading changes installed package versions.
15. Inspect service status, logs, configuration, dependencies, permissions, and resource availability before changing anything.
16. Check the configured resolver path, hosts file, lookup results, and network path; interpret each test carefully.
17. Risk depends on the service, configuration, exposure, authentication, and controls.
18. An attacker might change what a privileged process executes.
19. Quoting usually prevents unwanted word splitting and glob expansion.
20. Review usernames, hostnames, addresses, tokens, keys, private paths, and log content.

## 10.5 Scenarios to explain in your own words

1. A command says “Permission denied.” What evidence would you collect before using `sudo`?
2. A machine has free memory but feels slow. What other resources or causes would you inspect?
3. A website name fails to resolve, but an IP address works. What layer would you investigate?
4. A service listens on a port. What else do you need to know before calling it a security issue?
5. A script works manually but fails in a scheduled task. Which environment differences might matter?

## 10.6 Further study

Use these primary references to go deeper:
- Linux manual pages installed on your system: `man intro`, `man bash`, `man chmod`, `man find`.
- The Linux man-pages project: https://www.kernel.org/doc/man-pages/
- The Linux kernel documentation: https://docs.kernel.org/
- GNU Bash manual: https://www.gnu.org/software/bash/manual/
- Debian documentation: https://www.debian.org/doc/
- Kali Linux documentation: https://www.kali.org/docs/
- Filesystem Hierarchy Standard: https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html

Official documentation can be more detailed than a beginner chapter. Use it when you need exact behavior for a specific version.

## 10.7 Final self-check

Before moving to the next chapter, explain these without looking at your notes:
- how a command gets from the shell to a running program;
- how a path is resolved;
- how Linux checks basic file permissions;
- why least privilege matters;
- how you would inspect a process, service, and listening socket;
- how to write and safely run a small Bash script.

If an answer is unclear, revisit that section and run the corresponding safe lab.
