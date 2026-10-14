# 4. How Programs Run

A program is a set of instructions. To run it, the operating system prepares it for execution and the CPU runs its instructions.

```text
Program saved on storage
          |
          v
Operating system loads it
          |
          v
Memory and runtime state are prepared
          |
          v
CPU runs instructions
          |
          v
Result: screen, file, or network activity
```

The exact steps depend on the programming language. A compiled program may already have been translated into machine code. A Python script is normally run by the Python interpreter.

## The instruction cycle

A simple model is **fetch, decode, execute**:

1. **Fetch:** get the next instruction.
2. **Decode:** work out what the instruction asks for.
3. **Execute:** perform the operation.

Modern processors do more internally and can overlap or reorder work while preserving the program's expected behaviour. The three steps are a learning model, not a full description of modern CPU design.

## Example

```python
a = 2
b = 3
total = a + b
print(total)
```

Output:

```text
5
```

Python handles the code, and the computer performs the operations needed to produce the result. The CPU does not understand the human meaning of the variable name `total`; that name belongs to the programming language and runtime.

## Why this matters in security

When investigating a suspicious program, useful questions include:

- Which process is running it?
- Which files does it read or change?
- Does it start other processes?
- Does it make network connections?
- Which user permissions does it have?

These questions help connect code to its behaviour.

## Quick check

1. What are the three steps in the simple instruction-cycle model?
2. Why is running an unknown program risky?
3. What could you inspect when investigating a suspicious process?

## Behind the scenes: starting a Linux program

A simplified Linux execution path can look like this:

1. A shell reads the command you typed.
2. The shell locates the program according to the command and environment.
3. The shell requests process creation or execution using operating-system interfaces.
4. The kernel checks access and prepares the program's memory mappings.
5. The dynamic loader may load shared libraries when needed.
6. The program begins at its entry point and executes instructions.
7. The program may request files, network sockets, or other services through system calls.

Exact details depend on whether the command is built into the shell, a script, a dynamically linked executable, or something else.

## Compiled and interpreted code

A compiler translates source code into another form, often machine code. An interpreter executes a program according to the language's rules. Many languages use a mixture of compilation, bytecode, runtime services, and just-in-time compilation.

The important idea is that source code is not usually sent directly to the CPU as written. Some tool or runtime must turn it into operations the machine can execute.

## Observe a command safely

```bash
command -v ls
file "$(command -v ls)"
```

`command -v` shows how the shell resolves a command in common cases. `file` identifies the type of the resulting path. These are observations, not a guarantee that a file is trustworthy.

## Real-world security scenario: suspicious execution

If an unfamiliar process appears, useful evidence can include:

- The full command line.
- The executable's path and file hash.
- The user that owns the process.
- The parent process that launched it.
- Files opened and network connections made.
- Relevant system and application logs.

No single item proves malicious activity. Investigators combine evidence and compare it with normal system behaviour.

## Common mistakes

- Assuming every shell command is a standalone executable; some are shell built-ins or aliases.
- Running unknown code to see what it does.
- Believing a familiar process name proves authenticity.
- Confusing source code with the machine instructions that execute.
