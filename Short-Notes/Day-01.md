# 100-Day Pentesting Challenge
## Phase 1: Linux Fundamentals — Days 1–3

> **Why Linux?** Almost every server, IoT device, and pentesting tool runs on Linux.
> If you can't navigate Linux, you can't hack. Master this phase before moving forward.

---

# Day 1: Linux CLI Basics

> **Goal:** Get comfortable moving around the terminal. This is your cockpit — learn every button.

---

## Core Commands

| Command | What It Does | Example |
|---|---|---|
| `pwd` | Print working directory — shows your current location | `pwd` |
| `ls` | List files and directories | `ls` |
| `ls -l` | Long format listing — shows permissions, owner, size, date | `ls -l /home/user` |
| `ls -a` | Show all files including hidden ones (dotfiles) | `ls -la /var/log` |
| `ls -lh` | Long format with human-readable file sizes (KB, MB) | `ls -lh /var/log` |
| `cd` | Change directory | `cd /etc/nginx` |
| `cd ..` | Go one directory up | `cd ..` |
| `cd ~` | Jump straight to your home directory | `cd ~` |
| `cd -` | Go back to the previous directory | `cd -` |
| `mkdir` | Create a new directory | `mkdir recon_notes` |
| `mkdir -p` | Create nested directories in one command | `mkdir -p tools/web/burp` |
| `touch` | Create an empty file or update its timestamp | `touch notes.txt` |
| `cat` | View entire file content at once | `cat /etc/hosts` |
| `less` | View file page by page (use `q` to quit) | `less /var/log/syslog` |
| `more` | Similar to less but simpler — scroll-only forward | `more /etc/passwd` |
| `head` | Show first N lines of a file | `head -n 20 file.log` |
| `tail` | Show last N lines of a file | `tail -n 20 file.log` |
| `tail -f` | Follow a file in real-time (great for live logs!) | `tail -f /var/log/auth.log` |
| `echo` | Print text to the terminal or into a file | `echo "target=10.10.10.1"` |
| `printf` | Like echo but with more formatting control | `printf "IP: %s\n" 10.10.10.1` |
| `history` | Show all previously run commands | `history` |
| `history \| grep ssh` | Search your history for a specific command | `history \| grep nmap` |
| `!!` | Re-run the last command | `!!` |
| `!<n>` | Re-run command number n from history | `!42` |
| `alias` | Create a shortcut for a command | `alias ll='ls -lah'` |
| `unalias` | Remove a previously set alias | `unalias ll` |
| `nano` | Beginner-friendly terminal text editor | `nano config.txt` |
| `vi / vim` | Powerful terminal editor (steeper learning curve) | `vim payload.py` |
| `clear` | Clear the terminal screen | `clear` |
| `reset` | Fully reset the terminal (fixes broken displays) | `reset` |
| `man` | Open the manual page for any command | `man nmap` |
| `--help` | Quick usage info for most commands | `nmap --help` |
| `type` | Shows whether a command is built-in, alias, or binary | `type ls` |
| `which` | Find the full path of an executable | `which python3` |
| `whatis` | One-line description of any command | `whatis grep` |
| `date` | Show or set the system date and time | `date` |
| `cal` | Display a calendar | `cal` |
| `whoami` | Print your current username | `whoami` |
| `uname -a` | Show all system info (kernel, architecture, hostname) | `uname -a` |
| `hostname` | Show or change the system's hostname | `hostname` |

---

## Additional Details

### 1. Nano Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+S` | Save current file |
| `Ctrl+O` | Save As (write to a new name) |
| `Ctrl+X` | Close and exit nano |
| `Ctrl+K` | Cut the current line |
| `Ctrl+U` | Paste the cut line |
| `Ctrl+W` | Search inside the file |
| `Ctrl+G` | Open nano help |

---

### 2. Flags — How to Supercharge Any Command

Flags modify what a command does. They come in two forms:

```bash
ls -l          # short flag (single hyphen)
ls --all       # long flag (double hyphen)
ls -lah        # combining multiple flags together
```

> **Pentest Tip:** Always run `man <command>` or `<command> --help` when you're unsure.
> Flags like `-r`, `-v`, and `-n` mean different things in different commands.

---

### 3. Tab Completion & Keyboard Shortcuts

```bash
Tab            # Auto-complete file/command names
Ctrl+C         # Kill the running process
Ctrl+Z         # Suspend the running process (use bg to resume)
Ctrl+D         # Logout / exit the terminal
Ctrl+L         # Same as clear
Ctrl+A         # Jump to start of line
Ctrl+E         # Jump to end of line
Ctrl+R         # Reverse search through command history
```

---

> **Pro Tip:** Pipe commands together using `|`. For example:

```bash
history | grep ssh
```

This is one of the most commonly used techniques during pentesting to filter command output.
