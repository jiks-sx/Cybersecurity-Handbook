# 100-Day Pentesting Challenge
## Phase 1: Linux Fundamentals — Day 3

---

# Day 3: Advanced Linux Commands

> **Goal:** Learn the power tools. These are what real pentesters, sysadmins, and CTF players use daily.

---

## Core Commands

| Command | What It Does | Example |
|---|---|---|
| `grep` | Search for text patterns inside files | `grep "error" app.log` |
| `grep -r` | Recursively search all files in a directory | `grep -r "password" /var/www/` |
| `grep -i` | Case-insensitive search | `grep -i "admin" config.php` |
| `grep -v` | Invert match — show lines that DON'T match | `grep -v "^#" /etc/ssh/sshd_config` |
| `grep -n` | Show line numbers with results | `grep -n "root" /etc/passwd` |
| `grep -E` | Use extended regular expressions (ERE) | `grep -E "^(root\|admin)" /etc/passwd` |
| `awk` | Field-based text processing and pattern scanning | `awk '{print $1}' access.log` |
| `awk -F` | Set a custom field separator | `awk -F: '{print $1}' /etc/passwd` |
| `sed` | Stream editor — find and replace text | `sed 's/old/new/g' file.txt` |
| `sed -i` | Edit file in place | `sed -i 's/admin/root/g' users.txt` |
| `sort` | Sort lines alphabetically or numerically | `sort usernames.txt` |
| `sort -u` | Sort and remove duplicates | `sort -u ips.txt` |
| `uniq` | Filter consecutive duplicate lines | `sort file.txt \| uniq` |
| `uniq -c` | Count occurrences of each unique line | `sort log.txt \| uniq -c \| sort -rn` |
| `cut` | Extract specific columns or characters | `cut -d: -f1 /etc/passwd` |
| `tr` | Translate or delete characters | `echo "hello" \| tr 'a-z' 'A-Z'` |
| `wc` | Count words, lines, or characters | `wc -l users.txt` |
| `diff` | Compare two files line by line | `diff original.conf modified.conf` |
| `xargs` | Build commands from standard input | `cat ips.txt \| xargs -I{} ping -c1 {}` |
| `tee` | Write output to both terminal and a file | `nmap 10.10.10.1 \| tee scan.txt` |
| `\| (Pipe)` | Pass output of one command to another | `ps aux \| grep apache` |
| `>` | Redirect output to a file (overwrite) | `echo "note" > file.txt` |
| `>>` | Append output to a file | `echo "note" >> file.txt` |
| `<` | Use a file as input | `sort < wordlist.txt` |
| `2>/dev/null` | Discard error messages | `find / -name "flag.txt" 2>/dev/null` |
| `2>&1` | Redirect errors to the same destination as output | `command > output.txt 2>&1` |
| `ps aux` | Show all running processes | `ps aux \| grep firefox` |
| `pgrep` | Find process IDs by name | `pgrep sshd` |
| `top` | Live process monitor | `top` |
| `htop` | Interactive process monitor | `htop` |
| `kill` | Send a signal to terminate a process | `kill -9 1234` |
| `killall` | Kill all processes by name | `killall firefox` |
| `pkill` | Kill processes by name pattern | `pkill -f "python"` |
| `jobs` | List background jobs | `jobs` |
| `bg` | Resume a suspended job in the background | `bg %1` |
| `fg` | Bring a background job to the foreground | `fg %1` |
| `nohup` | Run a command after terminal closes | `nohup ./scan.sh &` |
| `screen` | Terminal multiplexer | `screen -S mysession` |
| `tmux` | Modern terminal multiplexer | `tmux new -s pentest` |
| `tar` | Archive and compress files | `tar -xzvf backup.tar.gz` |
| `zip / unzip` | Create or extract ZIP archives | `unzip rockyou.zip` |
| `gzip / gunzip` | Compress or decompress `.gz` files | `gunzip file.txt.gz` |
| `base64` | Encode or decode Base64 | `echo "aGVsbG8=" \| base64 -d` |
| `xxd` | Display a hexadecimal dump | `xxd binary_file \| head` |
| `strings` | Extract printable strings from a file | `strings malware.exe` |
| `curl` | Transfer data using HTTP/HTTPS | `curl http://10.10.10.1/` |
| `wget` | Download files | `wget http://10.10.10.1/shell.php` |
| `nc (netcat)` | Listen, connect, and transfer data | `nc -lvnp 4444` |
| `ssh` | Secure Shell remote login | `ssh user@10.10.10.1` |
| `scp` | Securely copy files | `scp shell.php user@ip:/var/www/` |
| `netstat` | Display network connections (legacy) | `netstat -tulnp` |
| `ss` | Modern replacement for netstat | `ss -tulnp` |
| `ping` | Test network connectivity | `ping -c 4 8.8.8.8` |
| `traceroute` | Trace packet path | `traceroute 8.8.8.8` |
| `dig` | DNS lookup utility | `dig google.com A` |
| `nslookup` | Query DNS servers | `nslookup 10.10.10.1` |
| `arp` | Display or modify the ARP cache | `arp -a` |
| `ip a` | Show network interfaces | `ip a` |
| `ip r` | Display routing table | `ip r` |
| `ifconfig` | Configure/display interfaces (legacy) | `ifconfig eth0` |
| `systemctl` | Manage system services | `systemctl status ssh` |
| `journalctl` | Read system logs | `journalctl -xe` |
| `crontab -l` | List cron jobs | `crontab -l` |
| `crontab -e` | Edit cron jobs | `crontab -e` |
| `cat /etc/crontab` | View system cron jobs | `cat /etc/crontab` |
| `env` | Display environment variables | `env` |
| `export` | Set environment variables | `export PATH=$PATH:/opt/tools` |
| `uptime` | Show system uptime | `uptime` |
| `free -h` | Display memory usage | `free -h` |
| `lscpu` | Show CPU information | `lscpu` |
| `lsof` | List open files and ports | `lsof -i :80` |

---

## Additional Details

### 1. `ps aux` — Explained

The most commonly used process inspection command:

```text
USER       PID  %CPU  %MEM    VSZ   RSS  TTY  STAT  START  TIME  COMMAND
root         1   0.0   0.1  22580  5400  ?    Ss    09:00  0:01  /sbin/init
```

- `a` = Show processes for all users
- `u` = Display user information
- `x` = Include processes without a controlling terminal

> **Pentest Use:** `ps aux | grep -v "grep"` helps identify running services and processes that may be useful during enumeration or privilege escalation.

---

### 2. `grep` — A Pentester's Best Friend

```bash
grep "password" config.php              # Find plaintext passwords
grep -r "DB_PASSWORD" /var/www/         # Search recursively
grep -v "^#" /etc/sshd_config           # Hide commented lines
grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}"   # Find IP addresses
grep -oP '(?<=password=)[^\s]+'         # Extract password values
```

---

### 3. `ssh` — Secure Shell

```bash
ssh user@10.10.10.1
ssh -p 2222 user@10.10.10.1
ssh -i id_rsa user@10.10.10.1
ssh -L 8080:127.0.0.1:80 user@10.10.10.1
ssh -D 1080 user@10.10.10.1
```

> **Pentest Use:** SSH port forwarding allows access to internal services that are not directly reachable. This technique is commonly known as **SSH tunneling** and is widely used during pivoting.

---

### 4. `systemctl` — Service Management

```bash
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl enable nginx
systemctl disable nginx
systemctl status nginx
systemctl list-units --type=service --state=running
```

---

### 5. Cron Jobs — Scheduled Tasks

```bash
crontab -l
cat /etc/crontab
ls /etc/cron.d/
cat /var/log/syslog | grep CRON
```

> **Pentest Note:** Misconfigured cron jobs can sometimes be abused during privilege escalation if they execute writable scripts or binaries.

---

### 6. Useful One-Liners for CTFs & Recon

```bash
# Check for SUID binaries
find / -perm -4000 2>/dev/null

# Find writable directories
find / -writable -type d 2>/dev/null

# Extract unique IP addresses from a log
grep -oE "\b([0-9]{1,3}\.){3}[0-9]{1,3}\b" access.log | sort -u

# Decode a Base64 string
echo "aGVsbG8gd29ybGQ=" | base64 -d

# Listen for a reverse shell
nc -lvnp 4444

# Receive a file
nc -lvnp 9999 > received_file.txt

# Send a file
nc 10.10.10.1 9999 < file_to_send.txt
```

---

> **End of Days 1–3**

**Next Phase:** Networking Fundamentals — Understanding TCP/IP, ports, and protocols used in penetration testing.

---
