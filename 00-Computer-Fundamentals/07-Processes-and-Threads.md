# 7. Processes and Threads

## Program, process, and thread

These terms are related, but they do not mean the same thing.

- **Program:** instructions stored in a file or provided to a runtime.
- **Process:** a running instance of a program, with its own execution state and resources.
- **Thread:** a path of execution inside a process.

A process commonly has a process ID (PID), memory mappings, credentials, and one or more threads. Threads in the same process usually share much of the process's memory, but each thread has its own execution state and stack.

| Term | Simple way to think about it |
|---|---|
| Program | The instructions |
| Process | A running copy of the program |
| Thread | One execution path inside that running copy |

## Process states

A simplified process lifecycle looks like this:

```text
Created -> Ready -> Running -> Waiting
                    ^           |
                    |           |
                    +-----------+
                    |
                  Exited
```

Real operating systems use more states and scheduling rules. A process can wait for input/output, be paused by the scheduler, or terminate.

## Useful Linux commands

```bash
ps aux
top
pgrep -a ssh
pstree -p
```

- `ps aux` lists processes.
- `top` shows a live view of processes and resource use.
- `pgrep -a ssh` searches for matching processes and displays command lines.
- `pstree -p` shows parent-child relationships and PIDs.

A process name alone does not prove that a program is safe. Check its path, command line, parent process, user, and behaviour before deciding what it is.

## Security connection

Process inspection helps during troubleshooting and incident response. It can help identify unexpected programs, suspicious command lines, or a service running with more privileges than it needs.

## Quick check

1. Can one program have more than one process?
2. What is one difference between a process and a thread?
3. Which command would you use to view a process tree?

## Behind the scenes: launching a child process

On Unix-like systems, a process may create another process and then run a different program in it. Common interfaces include `fork()` and `execve()`. Shells use process-management mechanisms to launch many external commands and wait for their results.

A parent-child relationship is useful evidence during investigations because it can show how a process was started. However, a process tree alone does not prove whether activity is good or bad.

## Resource use

A process may use CPU, memory, files, and network sockets. High usage can be normal for a build, backup, scan, or data-processing job. It can also indicate a bug or denial-of-service condition.

Use a baseline and context:

```bash
ps -eo pid,ppid,user,%cpu,%mem,comm --sort=-%cpu | head
```

This displays selected process fields and sorts by CPU usage. Field availability can vary by `ps` implementation.

## Threads and race conditions

Threads share process resources, so they need ways to coordinate access to shared data. If the result depends on the exact timing of operations, a race condition may occur.

Race conditions can cause data corruption or security bugs. Synchronisation mechanisms such as locks and atomic operations help, but they must be used correctly.

## Real-world incident example

A server suddenly launches many child processes. Possible explanations include a legitimate worker pool, a misconfigured script, a software loop, or suspicious activity. Compare the parent process, user, command line, start time, logs, and expected workload before drawing a conclusion.

## Practical safety note

Do not terminate unfamiliar system processes just to test a command. If you are practising process management, use a harmless program you started yourself.
