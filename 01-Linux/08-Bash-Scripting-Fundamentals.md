# 8. Bash Scripting Fundamentals


## 8.1 Why write scripts?

A script stores a sequence of commands so the same task can be repeated consistently. Scripts are useful for collecting system information, processing logs, automating lab setup, and checking expected conditions.

Automation also multiplies mistakes. A script should have a clear purpose, predictable inputs, safe output paths, and error handling appropriate to the task.

## 8.2 Your first script

Create a file:

```bash
nano ~/linux-practice/system-summary.sh
```

Paste this script:

```bash
#!/usr/bin/env bash
set -u

printf '%s\n' '=== System summary ==='
printf 'User: '
id -un
printf 'Current directory: '
pwd
printf 'Kernel: '
uname -r
printf 'Date: '
date
```

Save in nano with `Ctrl+O`, press `Enter`, then exit with `Ctrl+X`.

The first line is a shebang. It tells the system which interpreter should run the script when executed directly. `/usr/bin/env bash` locates Bash through the environment's command search path.

`set -u` makes Bash treat expansion of an unset variable as an error. It is one useful safety setting, not a complete security solution.

Run it using Bash:

```bash
bash ~/linux-practice/system-summary.sh
```

Or mark it executable and run it directly:

```bash
chmod u+x ~/linux-practice/system-summary.sh
~/linux-practice/system-summary.sh
```

Only mark files executable when you understand their contents and purpose.

## 8.3 Variables and quoting

```bash
name="Linux learner"
printf 'Hello, %s\n' "$name"
```

There must be no spaces around the `=` in a simple Bash assignment. Double quotes around `"$name"` preserve it as one argument even if the value contains spaces.

Prefer `printf` when you want predictable formatted output. `echo` has differences across shells and implementations for some options and escape sequences.

## 8.4 Conditions and exit statuses

```bash
#!/usr/bin/env bash

if grep -q 'root' /etc/passwd; then
    printf '%s\n' 'The text was found.'
else
    printf '%s\n' 'The text was not found.'
fi
```

`grep -q` suppresses matching lines and uses an exit status to indicate whether a match was found. In this example, it checks a local system file. The presence of the word `root` in that file is expected and is not a security finding.

## 8.5 Loops

```bash
for item in one two three; do
    printf 'Item: %s\n' "$item"
done
```

A loop repeats a block for each item. When processing filenames, avoid assuming that splitting text on spaces is safe: filenames can contain spaces and other unusual characters. Use robust patterns such as null-delimited input when a task requires processing arbitrary filenames.

## 8.6 Arguments

```bash
#!/usr/bin/env bash

if [ "$#" -ne 1 ]; then
    printf 'Usage: %s NAME\n' "$0" >&2
    exit 2
fi

printf 'Hello, %s\n' "$1"
```

- `$0` is the script name in common Bash contexts.
- `$#` is the number of positional arguments.
- `$1` is the first argument.
- `exit 2` ends the script with a non-zero status.

Validate input before using it in paths or commands. Quoting helps prevent word splitting and glob expansion, but it does not automatically prevent every kind of unsafe behavior.

## 8.7 Safer script habits

- Quote variable expansions unless you have a specific reason not to.
- Validate arguments and handle missing inputs.
- Use clear output and meaningful exit statuses.
- Avoid running the whole script as root unless required.
- Avoid `eval` for ordinary argument handling.
- Do not hard-code passwords, API keys, or tokens in scripts.
- Test with harmless sample data before real data.
- Be cautious with recursive deletion, broad wildcards, and commands that modify many files.

`set -e` is often used to stop on some command failures, but its behavior has exceptions and can surprise new Bash users. Do not treat it as a substitute for deliberate error handling.

## 8.8 Mini exercise

Modify the first script to display:
- the distribution name from `/etc/os-release`;
- the current kernel version;
- available disk space for `/`.

Use read-only commands. Explain what each line does and record any differences between your Kali VM and another Linux distribution.

## 8.9 Summary

Bash scripts make repeatable work easier, but scripts can also repeat mistakes quickly. Learn quoting, arguments, conditions, loops, exit statuses, and least privilege before automating security-sensitive tasks.
