# 2. Terminal, Shell, and Commands


## 2.1 Terminal vs shell vs command

These terms are related, but they are not interchangeable.

- **Terminal emulator:** The application window that displays text and accepts keyboard input.
- **Shell:** The program that reads commands, expands variables and patterns, and starts programs. Bash and Zsh are common examples.
- **Command:** An instruction handled by the shell. It might be a built-in, an alias, a function, or an external executable.
- **Process:** A running instance of a program.

In Kali, your default shell may be Zsh rather than Bash. Check it with:

```bash
echo "$SHELL"
ps -p $$ -o comm=
```

`$SHELL` often reports your configured login shell. `ps` reports the command name for the current shell process. They can differ in some setups.

## 2.2 A command's basic structure

```bash
command [options] [arguments]
```

Example:

```bash
ls -lh /var/log
```

- `ls` is the command.
- `-l` requests a long listing.
- `-h` asks for human-readable sizes where supported.
- `/var/log` is the target path.

Options can have short forms such as `-l` or long forms such as `--all`. Not every command uses the same option syntax.

## 2.3 Learn commands through help

```bash
man ls
ls --help
type ls
command -v ls
```

- `man` opens a manual page, if installed.
- `--help` commonly prints a short usage guide.
- `type` tells you how the shell interprets a name.
- `command -v` helps identify the command the shell would run.

Manual pages are often the best way to confirm exact behavior on your machine. Press `q` to leave many `man` and `less` views.

## 2.4 Essential navigation commands

```bash
pwd
ls
ls -la
cd /etc
pwd
cd ~
pwd
cd -
```

- `pwd`: print working directory.
- `ls`: list directory entries.
- `ls -la`: include hidden entries and show a long listing.
- `cd`: change the shell's current directory.
- `~`: usually expands to your home directory.
- `cd -`: return to the previous working directory in common shells.

A name beginning with a dot, such as `.bashrc`, is hidden by normal `ls` output; it is not encrypted or inherently protected.

## 2.5 Paths and quoting

A path is a location in the filesystem. A path beginning with `/` is absolute. A path not beginning with `/` is usually interpreted relative to the current working directory.

```bash
cat /etc/os-release
cat ../notes.txt
```

Spaces and special characters can affect how a shell parses text. Quote paths that contain spaces:

```bash
ls "my lab notes"
```

Single quotes generally prevent shell expansion inside the quoted text. Double quotes still allow certain expansions such as `$HOME`.

Do not assume quotes make an unsafe command safe. They only affect parsing.

## 2.6 Standard input, output, and errors

Many programs use three standard streams:
- **stdin (0):** input;
- **stdout (1):** normal output;
- **stderr (2):** error messages.

Redirect normal output to a file:

```bash
uname -a > system-info.txt
```

Append output instead of replacing the file:

```bash
date >> system-info.txt
```

Redirect errors:

```bash
ls /path/that/does-not-exist 2> errors.txt
```

Combine standard output and standard error in Bash:

```bash
some-command > output.txt 2>&1
```

Be careful with `>`: it normally replaces the destination file's contents. Check the path before running a redirection.

## 2.7 Pipes

A pipe (`|`) sends one command's standard output to another command's standard input.

```bash
ps aux | less
ip address | less
```

Example: search a list of processes for a term:

```bash
ps aux | grep -i ssh
```

This may also show the `grep` process itself, and a match does not prove a process is malicious or legitimate. Treat it as a first inspection step, not a complete investigation.

## 2.8 Exit status

A command usually returns an exit status. Zero commonly means success; a non-zero value commonly indicates an error or another result.

```bash
true
echo $?

false
echo $?
```

`$?` is the exit status of the most recently completed command. Run `echo $?` immediately after the command you want to inspect.

## 2.9 Command history and tab completion

- Press `Tab` to complete paths and command names where supported.
- Use the up/down arrows to move through history.
- `history` may display recent commands.

History can contain tokens, hostnames, private paths, or passwords accidentally typed into commands. Avoid putting secrets directly into command-line arguments, and do not share terminal screenshots without reviewing them.

## 2.10 Safe command habits

1. Read the manual before using an unfamiliar option.
2. Confirm the current directory with `pwd`.
3. Use read-only inspection commands before modification.
4. Quote paths containing spaces.
5. Review redirection targets before using `>` or `>>`.
6. Do not blindly run commands with `sudo`.
7. Avoid commands copied from untrusted pages, especially ones that download and immediately execute scripts.
8. Keep notes of the command, expected result, actual result, and any error.

## 2.11 Mini exercise

Run:

```bash
pwd
printf 'hello from Linux\n'
printf 'one\ntwo\nthree\n' | grep two
printf 'normal output\n' > practice-output.txt
cat practice-output.txt
```

Explain what each command does. Then inspect `practice-output.txt` with `ls -l` and `stat practice-output.txt`. Remove your practice file only after confirming that you created the correct file:

```bash
rm -- practice-output.txt
```

`rm` deletes a directory entry and can be destructive when pointed at the wrong path. The `--` marks the end of options for many commands; it is useful when a filename might begin with a hyphen.

## 2.12 Summary

The terminal is the interface, the shell parses commands, and programs perform work. Learning quoting, paths, streams, pipes, exit statuses, help systems, and safe habits makes Linux use more reliable and easier to troubleshoot.
