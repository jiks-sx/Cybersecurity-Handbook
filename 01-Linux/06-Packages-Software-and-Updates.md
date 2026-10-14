# 6. Packages, Software, and Updates


## 6.1 What is a package manager?

A package manager installs, updates, removes, and tracks software using packages and repository metadata. It helps maintain dependencies and makes software installation more repeatable than manually downloading random files.

Kali and Debian-based systems commonly use APT and `.deb` packages. Fedora commonly uses DNF and RPM packages. Commands differ across distributions.

## 6.2 APT: inspect before changing

On a Debian-based system:

```bash
apt --version
apt list --installed | less
apt search curl
apt show curl
```

These commands inspect package-management information. Package names, versions, and available metadata depend on configured repositories.

Updating repository metadata is different from upgrading installed software:

```bash
sudo apt update
```

This refreshes local package indexes from configured repositories. It does not, by itself, upgrade every installed package.

A system upgrade can change many packages and may affect compatibility. Before upgrading, review the proposed changes, ensure you have a way to recover, and avoid doing it during a critical task.

## 6.3 Install and remove carefully

Example commands for a Debian-based lab:

```bash
sudo apt install tree
tree ~/linux-practice
```

Only install software you need and trust. Read the package summary and review what will be installed. To remove a package, inspect the proposed actions before confirming. Removing a package can affect dependent tools or workflows.

Never add an unknown repository or run an unverified installation script just because a tutorial tells you to.

## 6.4 Where software comes from

A package manager usually retrieves software from configured repositories. Trust depends on repository configuration, signing keys, package metadata, the distribution's update process, and the security of the distribution and its maintainers.

Third-party repositories can expand your options but also expand your trust boundary. Adding one may allow it to supply software or updates. Use official documentation and verify repository instructions carefully.

## 6.5 Updates and security

Updates can fix vulnerabilities, bugs, and compatibility issues. However:
- an update may require a restart or service restart;
- an update can introduce regressions;
- a machine may use a pinned or intentionally older package;
- not every vulnerability is fixed by updating one package;
- configuration and exposure still matter.

For important systems, use a planned maintenance process: assess impact, back up important data, test when practical, apply updates, validate services, and document the result.

## 6.6 Verify installed software

```bash
command -v python3
python3 --version
dpkg -l | grep -i python3
```

These commands answer different questions:
- `command -v` asks how the shell resolves a command name.
- `--version` reports the selected program's version.
- `dpkg -l` lists package database information on Debian-based systems.

A command may come from a manually installed program rather than a package, so package listings are not a complete inventory of everything executable on the machine.

## 6.7 Common mistakes

- Confusing repository metadata refresh with software upgrade.
- Installing commands from untrusted scripts.
- Adding repositories without understanding the trust implications.
- Ignoring proposed dependency removals.
- Assuming every system uses APT.
- Assuming a successful update means the whole system is secure.

## 6.8 Summary

Package managers make software installation and maintenance more controlled, but repository trust and change management still matter. Inspect package information, understand the difference between metadata updates and upgrades, and review changes before confirming them.
