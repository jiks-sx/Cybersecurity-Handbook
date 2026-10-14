# 11. Practical Lab: Explore Linux

Use your own Linux computer or an authorised practice VM. The commands in this lab are mainly for viewing information and should not make system changes.

## Lab 1: Identify the operating system

```bash
cat /etc/os-release
uname -a
uname -r
```

Look for your distribution name and kernel version. `uname -r` prints the kernel release.

## Lab 2: Inspect the CPU

```bash
lscpu
```

Look for architecture, CPU model, logical CPU count, and core information. Details can differ on virtual machines.

## Lab 3: Inspect memory

```bash
free -h
head -n 20 /proc/meminfo
```

`free -h` gives a readable memory summary. Linux uses some RAM for caches, so do not assume all memory shown as used is permanently unavailable.

## Lab 4: Inspect storage

```bash
lsblk -f
df -hT
du -sh "$HOME"
```

- `lsblk -f` shows block devices and filesystem information.
- `df -hT` shows mounted filesystem usage and type.
- `du -sh "$HOME"` estimates the size of your home directory.

These commands answer different questions. `df` reports filesystem space; `du` totals files found under a directory.

## Lab 5: Inspect processes

```bash
ps aux --sort=-%mem | head
ps aux --sort=-%cpu | head
pstree -p
```

Look at which processes use the most memory or CPU. Do not kill a process just because you do not recognise its name. Investigate it first.

## Lab 6: Check your user and file permissions

```bash
id
pwd
ls -ld "$HOME"
ls -l /etc/passwd /etc/shadow
```

`id` shows your user and group IDs. On many Linux systems, `/etc/shadow` has restrictive permissions. Do not change its permissions for this lab.

## Lab 7: View network configuration

```bash
ip -brief address
ip route
```

These commands show network interfaces, addresses, and routing information. We will study the details in the networking chapter.

## Lab notes

Record observations in your own notes. Before publishing screenshots or output, remove public IP addresses, usernames, hostnames, serial numbers, and other details you do not want to share.

```text
Date:
Linux distribution:
Kernel version:
CPU architecture:
Logical CPU count:
RAM:
Root filesystem usage:
One process I looked at:
One thing I learned:
One question I still have:
```

## Lab safety

Do not paste unknown commands into a terminal and run them as root without understanding them. This lab does not require disk-wiping commands, permission changes on system files, or scans against systems you do not own.

## How to record useful evidence

For every command, write down:

1. **Question:** What were you trying to learn?
2. **Command:** What did you run?
3. **Observation:** Which output matters?
4. **Meaning:** What does the output suggest?
5. **Limit:** What can the command not prove?
6. **Next step:** What would you check if something looked unusual?

Example:

```text
Question: Which kernel release is this system using?
Command: uname -r
Observation: [write your own output]
Meaning: This is the reported kernel release.
Limit: This alone does not tell me whether the system is fully patched.
Next step: Check the distribution's update status and security advisories.
```

## More read-only commands

```bash
uptime
whoami
id
hostnamectl
mount | head
```

These can show uptime/load, current user, identity and groups, system details, and mounted filesystems. Output varies by distribution and configuration.

## Troubleshooting checklist

If a command fails:

- Read the error message carefully.
- Check spelling and whether the command is installed.
- Check the manual page with `man command-name`.
- Check whether the path exists.
- Check whether you have permission.
- Do not automatically add `sudo`; understand why permission is needed first.

## Protect your notes

Before posting lab output publicly, remove credentials, access tokens, public IPs where sensitive, private hostnames, serial numbers, and any information that could identify a private system. Use placeholders in public writeups when possible.
