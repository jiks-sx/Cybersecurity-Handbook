# 5. Processes, Services, and Jobs


## 5.1 What is a process?

A process is a running instance of a program. It has a process ID (PID), credentials, memory mappings, open file descriptors, and other kernel-managed state. The exact details depend on the operating system and program.

```bash
ps
ps aux | less
top
```

- `ps` reports selected process information.
- `ps aux` is a common process listing form on Linux.
- `top` displays changing process and resource information; press `q` to exit.

A process using high CPU is not automatically malicious. It may be doing useful work, stuck in a loop, or responding to a heavy workload.

## 5.2 Parent and child processes

Programs can create child processes. A shell often starts a command as a child process, waits for it, and then displays another prompt when it finishes. Some programs start background processes or use other process-management techniques.

```bash
sleep 30 &
jobs
```

The `&` asks the shell to start the command in the background. `jobs` lists jobs managed by the current shell. This is a temporary practice process; it exits after the specified time.

## 5.3 Signals and stopping processes

Signals are notifications sent to processes. `SIGTERM` requests termination; a process may handle it and clean up. `SIGKILL` cannot be caught or ignored and stops a process abruptly, so it should not be the first choice.

For your own practice process:

```bash
sleep 300 &
jobs
```

Note the PID or job number, then use the shell's job-control features to stop the practice process. Do not kill processes just because their names look unfamiliar. Identify the process, owner, executable, parent, and purpose first.

## 5.4 Services and systemd

A service is commonly a background program that provides a system function. Many Linux distributions use `systemd` to manage startup and services, but not every Linux system uses it.

Read-only inspection on a system with systemd:

```bash
systemctl --type=service --state=running
systemctl status ssh
```

The SSH service name and availability depend on the distribution and configuration. `status` may reveal logs or paths; review them before sharing output.

Avoid enabling, disabling, starting, or stopping services on a machine you do not administer. Changes can interrupt remote access or essential system functions.

## 5.5 Resource usage

```bash
free -h
uptime
df -h
ps -eo pid,ppid,user,%cpu,%mem,comm --sort=-%cpu | head
```

These commands give different views:
- memory and swap summary;
- uptime and load averages;
- filesystem space;
- a process listing sorted by CPU use.

Load average is not the same as CPU percentage. It reflects the average number of tasks runnable or waiting in uninterruptible states over time; interpretation depends on CPU count and workload.

## 5.6 Logs and investigation

Logs can help explain what a service was doing, but they are incomplete evidence by themselves. On a system with systemd, `journalctl` can inspect the journal:

```bash
journalctl -n 50 --no-pager
```

Access may be restricted, and the output may contain sensitive data. Do not post logs publicly without reviewing and redacting private details.

## 5.7 Security investigation checklist

When a process looks suspicious, collect facts before acting:
1. What is the PID and parent PID?
2. Which account owns it?
3. What executable or command line is shown?
4. Which files or network sockets does it use?
5. When did it start?
6. Is it expected on this system?
7. Do logs or package metadata help explain it?

A process name alone is weak evidence because names can be misleading. Avoid deleting files or killing processes before understanding the consequences, especially on production systems.

## 5.8 Summary

Processes run programs; services provide background functions; shell jobs are tasks managed by your current shell. Learn to inspect first, understand resource output, and treat process termination and service changes as potentially disruptive actions.
