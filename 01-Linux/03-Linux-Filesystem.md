# 3. Linux Filesystem

## 3.1 The filesystem tree

Linux presents files and directories through a tree that begins at `/`, called the root directory. This is different from the root user's home directory, which is often `/root`.

Common directories include:

| Path | Typical purpose |
|---|---|
| `/` | Start of the filesystem tree |
| `/home` | Home directories for regular users |
| `/root` | Root user's home directory |
| `/etc` | System-wide configuration |
| `/var` | Variable data, often including logs and caches |
| `/tmp` | Temporary files; exact cleanup policy varies |
| `/usr` | Many user-space programs, libraries, and shared data |
| `/bin`, `/sbin` | Essential commands on many systems; may be links into `/usr` |
| `/dev` | Device interfaces exposed as filesystem entries |
| `/proc` | Virtual view of process and kernel information |
| `/sys` | Kernel and device information/control interfaces |
| `/mnt`, `/media` | Common mount points |
| `/opt` | Often used for optional third-party software |

These are conventions, not guarantees that every distribution uses identical layouts.

## 3.2 Absolute and relative paths

```bash
pwd
ls /etc
ls ../
```

- Absolute path: starts from `/`, such as `/etc/hosts`.
- Relative path: starts from the current directory, such as `../notes.txt`.
- `.` means the current directory.
- `..` means the parent directory.

The shell resolves relative paths using its current working directory. A program can also choose to work with a different directory internally.

## 3.3 Inspect files without changing them

```bash
file /etc/hosts
stat /etc/hosts
head -n 10 /etc/hosts
tail -n 10 /etc/hosts
less /etc/hosts
```

- `file` makes a best-effort identification based on file contents and metadata.
- `stat` reports metadata such as size, permissions, ownership, and timestamps.
- `head` and `tail` show the beginning or end of a file.
- `less` allows scrolling through text.

A filename extension does not prove a file's actual type. A file named `image.jpg` could contain something else.

## 3.4 Create and organize a practice directory

```bash
mkdir -p ~/linux-practice/notes
cd ~/linux-practice
pwd
touch notes/day-01.txt
printf 'First Linux practice note\n' > notes/day-01.txt
cat notes/day-01.txt
```

`mkdir -p` creates missing parent directories. `touch` creates an empty file if it does not exist, or updates timestamps on an existing file. The `>` redirection replaces existing contents, so do not use it on a file whose contents you need to keep.

## 3.5 Copy, move, and remove

```bash
cp notes/day-01.txt notes/day-01-copy.txt
mv notes/day-01-copy.txt notes/day-01-review.txt
ls -l notes
```

Use these carefully:
- `cp` copies files or directories when used with the appropriate options.
- `mv` moves or renames entries.
- `rm` removes directory entries.
- `rmdir` removes empty directories.

Before deleting, inspect the exact target:

```bash
pwd
ls -ld -- notes/day-01-review.txt
```

Avoid using broad recursive deletion commands unless you fully understand the path and have verified the target. Do not experiment with destructive commands on system directories.

## 3.6 Hidden files, links, and metadata

Names beginning with `.` are hidden by convention in common directory listings:

```bash
ls -la ~
```

A symbolic link stores a path reference to another location. A hard link is another directory entry referring to the same underlying file object on filesystems that support it. Their behavior differs when the original name is removed or the target moves.

```bash
ln -s notes/day-01.txt notes/current-note.txt
ls -l notes
readlink notes/current-note.txt
```

Create links only in your practice directory for this exercise.

## 3.7 Mounts and storage

Linux can attach a filesystem to a directory called a mount point. A USB drive, virtual disk, network filesystem, or separate disk partition may be mounted into the tree.

```bash
df -h
findmnt
lsblk
```

- `df -h` shows filesystem capacity and usage.
- `findmnt` shows mounted filesystems.
- `lsblk` lists block devices and their relationships.

Do not mount, format, or modify storage devices just to explore these commands. Incorrectly identifying a device can cause data loss.

## 3.8 Real-world security connection

Configuration files, logs, SSH keys, application data, and temporary files can expose sensitive information if permissions are too broad. During an authorized audit, ask:
- Is this file supposed to be readable by all users?
- Does it contain secrets or personal data?
- Who owns it, and which groups can access it?
- Is a writable directory used by a privileged program?
- Are logs protected but still accessible to the people who need them?

Never assume that a file is safe to disclose simply because it is readable on your own machine.

## 3.9 Common mistakes

- Confusing `/` with `/root`.
- Assuming `..` means the parent of a fixed path rather than the current location.
- Overwriting a file with `>`.
- Using `rm` before verifying the target.
- Assuming a file extension proves its type.
- Assuming `/tmp` is always private or always cleared at a specific time.
- Treating all mounted storage as local and trustworthy.

## 3.10 Summary

The filesystem is a tree of names and mount points. Learn the purpose of common directories, distinguish absolute from relative paths, inspect metadata, and practice file operations in a dedicated directory before touching important data.
