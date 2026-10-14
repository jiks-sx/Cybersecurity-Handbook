# 8. Filesystems and Permissions

A filesystem organises files and directories on a storage device or volume. Examples include ext4, XFS, NTFS, and APFS.

A filesystem may store information such as names, ownership, timestamps, permissions, and references to file data.

## Common Linux directories

| Directory | Common use |
|---|---|
| `/` | Root of the directory tree |
| `/home` | Users' home directories |
| `/etc` | System configuration |
| `/var/log` | Many system and service logs |
| `/tmp` | Temporary files |
| `/dev` | Device interfaces |
| `/proc` | Process and kernel information |
| `/sys` | Device and kernel information |

These are common conventions; exact contents depend on the system.

## Reading Linux permissions

Example:

```text
-rwxr-xr--
```

The first character describes the file type. Here, `-` means a regular file. The next nine characters describe permissions for the owner, group, and others:

```text
rwx   r-x   r--
owner group others
```

- `r` means read.
- `w` means write.
- `x` means execute on a file. On a directory, it generally means you can pass through it.
- `-` means the permission is not granted.

Try these commands on your own system:

```bash
ls -l
stat notes.txt
```

On a test file you own, you can practise:

```bash
chmod 640 notes.txt
```

This sets owner read/write, group read, and no permissions for others. Filesystem features and other security controls can also affect access.

## Ownership and least privilege

Linux files commonly have an owning user and group. Access can also be affected by access-control lists, mount options, capabilities, and other security policies.

**Least privilege** means giving users and programs only the access they need. Do not run every command as root, and do not make files writable by everyone just to get around a permissions error.

Imagine a script that runs with administrator privileges. If an ordinary user can edit that script, they may be able to change what the privileged script does. This is why file ownership and permissions matter.

## Security connection

Weak permissions can expose secrets, allow unauthorised changes, or create opportunities for privilege escalation when privileged software trusts files that other users can modify.

## Quick check

1. What do `r`, `w`, and `x` mean?
2. Why should you avoid making files world-writable?
3. What does least privilege mean?

## Behind the scenes: checking access to a file

When a process requests a file, the operating system resolves the path and checks the process's identity and applicable access rules. On Linux, these may include owner/group/mode bits, access-control lists, capabilities, mount options, and additional security modules.

A file's permission string is therefore important, but it may not be the only rule affecting access.

## Symbolic and numeric permissions

The permission `rwxr-x---` can be written numerically as `750`:

- `rwx` = 4 + 2 + 1 = 7.
- `r-x` = 4 + 0 + 1 = 5.
- `---` = 0.

Another example is `640`: owner read/write, group read, and no permissions for others.

Be careful with recursive permission changes. A command such as `chmod -R` can change many files and may break a system if used on the wrong path.

## Links and file metadata

A symbolic link points to another path. A hard link is another directory entry for the same underlying file object on filesystems that support it. Timestamps and ownership can provide useful context during investigations, but timestamps alone can be changed and should not be treated as perfect proof.

## Real-world security scenario: writable script

Suppose a privileged scheduled task runs a script that an ordinary user can modify. The user may be able to change the script's behaviour, and the task may then execute the modified content with higher privileges. The safe fix is to review ownership, permissions, execution context, and the task configuration.

## Practical commands

```bash
namei -l /path/to/file
stat /path/to/file
getfacl /path/to/file
```

`namei -l` can show permissions along a path, `stat` displays file metadata, and `getfacl` displays ACL information when the tool and filesystem support it.

## Best practices

- Give files only the access they need.
- Avoid running applications as root without a clear reason.
- Protect secrets and configuration files.
- Check every directory in a path, not only the final file.
- Test permission changes on disposable files first.
