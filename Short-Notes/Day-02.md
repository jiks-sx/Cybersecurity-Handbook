# 100-Day Pentesting Challenge
## Phase 1: Linux Fundamentals — Day 2

---

# Day 2: File System & Permissions

> **Goal:** Understand how Linux organizes files and controls access.
> Privilege escalation and CTF flags are all about exploiting weak permissions.

---

## Core Commands

| Command | What It Does | Example |
|---|---|---|
| `cp` | Copy files or directories | `cp file.txt /backup/` |
| `cp -r` | Recursively copy an entire directory | `cp -r /etc/nginx /backup/nginx` |
| `mv` | Move or rename files | `mv old.txt new.txt` |
| `rm` | Remove a file (permanent!) | `rm malware_sample.txt` |
| `rm -rf` | Force-remove a directory and everything inside it | `rm -rf /tmp/old_scan/` |
| `rmdir` | Remove an **empty** directory only | `rmdir empty_folder/` |
| `file` | Detect the actual type of a file (don't trust extensions!) | `file suspicious_binary` |
| `find` | Search for files/directories with filters | `find / -name "*.conf" 2>/dev/null` |
| `find` (by perms) | Find files with specific permissions — key for privesc! | `find / -perm -4000 2>/dev/null` |
| `find` (by owner) | Find all files owned by a user | `find / -user root -writable 2>/dev/null` |
| `locate` | Fast file search using a cached database | `locate passwords.txt` |
| `updatedb` | Update the locate database manually | `sudo updatedb` |
| `which` | Show where an executable lives | `which python3` |
| `whereis` | Find binary, source, and man page locations | `whereis ncat` |
| `chmod` | Change file permissions | `chmod 755 exploit.sh` |
| `chmod` (symbolic) | Change permissions using symbols | `chmod u+x script.sh` |
| `chown` | Change file owner and group | `chown root:root shell.sh` |
| `chgrp` | Change only the group ownership | `chgrp developers app.conf` |
| `stat` | Detailed file metadata — timestamps, inode, permissions | `stat /etc/shadow` |
| `ls -li` | Show inode numbers alongside files | `ls -li /etc/` |
| `ln` | Create a hard link to a file | `ln file.txt hardlink.txt` |
| `ln -s` | Create a symbolic (soft) link | `ln -s /usr/bin/python3 python` |
| `readlink` | Show where a symlink points | `readlink -f /usr/bin/python` |
| `su` | Switch to another user account | `su root` |
| `su -` | Switch user with full login environment | `su - postgres` |
| `sudo` | Execute a command as superuser | `sudo apt update` |
| `sudo -l` | List what sudo commands you're allowed to run (key for privesc!) | `sudo -l` |
| `passwd` | Change a user's password | `passwd username` |
| `id` | Show your UID, GID, and group memberships | `id` |
| `groups` | List all groups you belong to | `groups` |
| `useradd` | Create a new user | `useradd -m hacker` |
| `usermod` | Modify a user account | `usermod -aG sudo hacker` |
| `userdel` | Delete a user | `userdel olduser` |
| `cat /etc/passwd` | View all user accounts on the system | `cat /etc/passwd` |
| `cat /etc/shadow` | View hashed passwords (needs root!) | `cat /etc/shadow` |
| `cat /etc/group` | View all groups on the system | `cat /etc/group` |
| `umask` | Show or set default permission mask for new files | `umask 022` |
| `du` | Check how much space a directory uses | `du -sh /var/log` |
| `df` | Check disk space on all mounted file systems | `df -h` |
| `mount` | List all mounted file systems | `mount` |
| `lsblk` | Show block devices (disks, partitions) | `lsblk` |

---

## Additional Details

### 1. The Filesystem Hierarchy Standard (FHS)

Linux organizes everything under a single root `/` — no drive letters like Windows.

| Directory | Purpose |
|---|---|
| `/` | Root of everything |
| `/bin` & `/sbin` | Essential system binaries and commands |
| `/etc` | System-wide configuration files (nginx, ssh, passwd, etc.) |
| `/var` | Variable data — logs, databases, mail |
| `/home` | Personal directories for regular users |
| `/tmp` | Temporary files — often cleared on reboot (commonly used for payloads during testing) |
| `/opt` | Optional or third-party software |
| `/dev` | Device files (disks, terminals, null, etc.) |
| `/proc` | Live kernel and process information (virtual filesystem) |
| `/root` | Home directory for the root user |
| `/usr` | User programs, libraries, and documentation |

> **Pentest Relevance:** `/tmp` is commonly writable by all users, making it a frequent location for temporary files and payloads during privilege escalation testing.

---

### 2. The Permission Architecture (UGO)

```text
 - rwx     r-x    r--  1  root  staff  4096  Jun 1  script.sh
 │ ↑↑↑     ↑↑↑    ↑↑↑
 │└──┬──┘└──┬──┘└──┬──┘
 │  Owner  Group  Others
 └─ File type (- = file, d = directory, l = symbolic link)
```

| Symbol | Permission | Octal Value |
|---|---|---|
| `r` | Read | 4 |
| `w` | Write | 2 |
| `x` | Execute | 1 |
| `-` | No permission | 0 |

**Common chmod values**

```bash
chmod 777 file    # Everyone can read, write, and execute (not recommended)
chmod 755 file    # Owner: full | Group & Others: read + execute
chmod 644 file    # Owner: read + write | Group & Others: read only
chmod 600 file    # Only owner can read + write (commonly used for SSH keys)
chmod 400 file    # Read-only for owner (e.g., id_rsa private key)
```

---

### 3. SUID / SGID — The Privilege Escalation Goldmine

```bash
# Find SUID binaries (run as owner, often root)
find / -perm -4000 -type f 2>/dev/null

# Find SGID binaries
find / -perm -2000 -type f 2>/dev/null

# Find world-writable files
find / -perm -o+w -type f 2>/dev/null
```

> **Why this matters:** If a SUID-root binary contains a vulnerability, it may allow privilege escalation to a root shell. Identifying misconfigured SUID/SGID binaries is a fundamental Linux privilege escalation technique.

--- 
