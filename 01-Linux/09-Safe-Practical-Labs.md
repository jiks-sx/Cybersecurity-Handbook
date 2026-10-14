# 9. Safe Practical Labs


These labs are designed for your own Kali Linux VM or another Linux system you are authorized to use. They focus on observation and harmless files, not attacking other systems.

## Lab 1 — Identify your environment

Run:

```bash
cat /etc/os-release
uname -r
whoami
id
pwd
```

Record:
- distribution and version;
- kernel version;
- current username and group IDs;
- current working directory.

**Questions**
1. Which details describe the distribution?
2. Which command reports kernel information?
3. Why are usernames and host details worth redacting before publishing screenshots?

## Lab 2 — Make a practice directory

```bash
mkdir -p ~/linux-practice/day-01
cd ~/linux-practice/day-01
printf 'Linux handbook practice\n' > notes.txt
cat notes.txt
stat notes.txt
file notes.txt
```

**Questions**
1. Which command created the directory?
2. Which command wrote text to the file?
3. What metadata did `stat` show?
4. Why can `>` overwrite content?

## Lab 3 — Understand permissions

```bash
cd ~/linux-practice/day-01
printf 'Only the owner should access this practice file.\n' > private.txt
chmod 600 private.txt
ls -l private.txt
stat private.txt
```

Explain the permission string and numeric mode. Do not use sensitive real data for this exercise.

## Lab 4 — Inspect processes

```bash
ps aux | less
free -h
uptime
```

Record one observation from each command. Do not terminate unfamiliar processes during this lab.

## Lab 5 — Inspect networking

```bash
ip address
ip route
ss -lntup
```

Record interface names, routes, and listening sockets that you understand. Redact internal IP addresses, usernames, and service details before sharing your notes.

## Lab 6 — Run your first script

Create `system-summary.sh` using the example in Chapter 8, then run:

```bash
bash ~/linux-practice/system-summary.sh
```

Describe each output line and why the script does not need administrator privileges.

## Lab report template

Use this template in your own notes:

```text
Lab:
Date:
Environment:
Goal:
Commands:
Expected result:
Actual result:
What I learned:
Errors and how I investigated them:
Security note:
Questions to research later:
```

## Safety rules

- Use only your own system or an explicitly authorized lab.
- Do not run destructive commands on system paths.
- Do not use `sudo` unless the lab explains why it is required.
- Do not expose secrets, tokens, internal addresses, or personal information in screenshots.
- Do not install unknown scripts or packages just to complete a lab.
- If a command behaves differently than expected, stop and investigate instead of adding random flags.
