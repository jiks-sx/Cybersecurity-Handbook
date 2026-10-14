# 4. Users, Groups, and Permissions


## 4.1 Why accounts and permissions exist

A multi-user operating system needs a way to decide which processes can read, change, execute, or administer resources. Linux associates processes with user and group identities. The kernel uses those credentials, along with other security mechanisms, when checking access.

A **user ID (UID)** identifies a user account to the operating system. A **group ID (GID)** identifies a group. Names are human-friendly labels mapped to numeric IDs.

Inspect your identity:

```bash
whoami
id
groups
```

## 4.2 Root and sudo

The root account normally has UID 0 and extensive administrative privileges. A regular user is more limited. `sudo` can allow an authorized user to run a command with elevated privileges, according to system policy.

```bash
sudo -l
```

This displays the commands your account is allowed to run through `sudo` on systems where the command is configured. It may ask for authentication. Do not try to bypass policy or use elevated access on systems without authorization.

Use the smallest privilege needed. Reading a normal text file usually does not require `sudo`; adding it everywhere increases the impact of a typo or unsafe command.

## 4.3 Read a permission listing

```bash
ls -l /etc/hosts
```

A typical permissions field may look like:

```text
-rw-r--r--
```

The first character indicates the entry type (for example, `-` for a regular file and `d` for a directory). The next nine characters are grouped into:
- owner permissions: `rw-`
- group permissions: `r--`
- other users' permissions: `r--`

Permission letters:
- `r`: read;
- `w`: write;
- `x`: execute for a file, or search/traverse for a directory;
- `-`: permission not granted by that permission bit.

Actual access can also be affected by ACLs, mount options, capabilities, mandatory access control systems, and other policy.

## 4.4 Numeric permissions

The common numeric values are:

| Permission | Value |
|---|---:|
| `r` | 4 |
| `w` | 2 |
| `x` | 1 |

Add the values for each owner/group/other class:
- `7` = `rwx`
- `6` = `rw-`
- `5` = `r-x`
- `4` = `r--`

For example, `chmod 640 file.txt` sets owner `rw-`, group `r--`, and others `---` on a regular file. This is only an example; choose permissions based on the file's purpose.

## 4.5 Directories behave differently

For directories:
- `r` generally allows listing names;
- `w` allows creating/removing/renaming entries, subject to other checks;
- `x` allows traversal/search when the path is known.

A user may be unable to open `/home/alex/notes.txt` if they lack search permission on one of the parent directories, even if the file itself appears readable.

Inspect every path component:

```bash
namei -l /etc/hosts
```

## 4.6 Change ownership and permissions safely

Only practice inside your own lab directory:

```bash
cd ~/linux-practice
printf 'private practice file\n' > private.txt
chmod 600 private.txt
ls -l private.txt
stat private.txt
```

`chmod 600` gives the owner read/write permissions and removes group/other permission bits. It does not encrypt the file, and it does not protect against root or every other access path.

Ownership changes commonly use `chown` and group changes can use `chgrp`. These often require administrative rights. Do not change ownership or permissions on system files as an experiment.

## 4.7 Special permission bits and ACLs

Linux filesystems may support special bits such as set-user-ID, set-group-ID, and the sticky bit. Their behavior depends on file type and context. These bits can affect execution identity or directory deletion behavior, so inspect them rather than changing them casually.

Access Control Lists (ACLs) can grant permissions beyond the basic owner/group/other model.

```bash
getfacl private.txt
```

The command may not be installed by default. If unavailable, consult your distribution's package documentation before installing it.

## 4.8 Security scenario: writable script

Imagine a privileged scheduled task runs a script owned by an administrator. If an untrusted user can modify that script or its parent directory, they may be able to influence what the privileged task executes. This is a configuration weakness because write access crosses a privilege boundary.

For an authorized audit, inspect:
- file owner and group;
- file permissions;
- permissions on every parent directory;
- who can modify the script or replace the path;
- whether the task really needs elevated privileges.

Do not attempt to exploit a real system. In a lab, demonstrate the concept using a harmless script you created yourself.

## 4.9 Common mistakes

- Believing `chmod 777` is a good general fix. It often grants unnecessary access.
- Thinking file permissions alone are the only access control.
- Forgetting that directory `x` means traversal/search.
- Running every command with `sudo`.
- Treating a readable secret as harmless.
- Changing ownership or permissions on system files without a recovery plan.

## 4.10 Summary

Users and groups identify processes; permissions help control access. Understand owner/group/other bits, numeric modes, directory traversal, `sudo`, and the possibility of additional controls. Good security means granting only the access that is actually needed.
