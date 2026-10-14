# 9. Boot Process, Firmware, and Security

When a computer starts, it needs to prepare its hardware and load an operating system. A simplified startup process is:

1. Power is applied.
2. Firmware starts and prepares important hardware.
3. Firmware selects a boot option.
4. A boot manager or bootloader helps load the operating system.
5. The kernel starts and prepares memory, drivers, and core features.
6. Services start and the system provides a login screen or terminal.

The exact process varies by device, firmware, operating system, and configuration.

## Important terms

- **Firmware:** low-level software used to start or control a device.
- **UEFI:** a modern firmware interface used by many PCs.
- **Bootloader:** helps start the operating system.
- **Secure Boot:** checks that boot components meet configured trust requirements.
- **TPM:** a security component that can help protect keys and support platform-integrity features.

## Secure Boot is useful, but not magic

Secure Boot can help prevent unauthorised boot components from loading, depending on configuration and trust settings. It does not prove that every application is safe or that the whole computer is free from vulnerabilities.

Firmware updates, boot settings, disk encryption, and physical access all affect system security.

## Cybersecurity connection

Boot security matters because an attacker who can change early startup components may be able to interfere with the system before normal security tools start. Defences should be combined rather than relying on one feature.

## Quick check

1. What is firmware?
2. What role does a bootloader play?
3. What does Secure Boot help check?

## Behind the scenes: a trusted boot chain

The startup process contains several stages, and each stage needs a way to decide what to run next. In a configured Secure Boot environment, firmware checks signatures against its trust databases before allowing a boot component to start. The bootloader may then load the kernel and related components.

The details depend on the platform, settings, keys, and operating system. Secure Boot does not automatically verify every file or application after startup.

## TPM and measured boot

A Trusted Platform Module (TPM) can protect keys and store measurements related to platform startup. Some systems use those measurements to help assess whether the boot state matches an expected configuration.

A TPM does not make a system invulnerable. Configuration, recovery processes, physical access, firmware quality, and the rest of the operating system still matter.

## Real-world scenario: lost laptop

A lost laptop creates risks if someone can access its storage. Full-disk encryption can reduce the risk of offline data access, especially when the device is powered off and keys are protected properly. It should be combined with strong authentication, updates, backups, and a plan for account recovery.

## Good practices

- Keep firmware updated using trusted vendor instructions.
- Review boot settings and restrict unneeded external boot options where appropriate.
- Use full-disk encryption on devices that hold sensitive information.
- Protect recovery keys separately from the device.
- Do not assume Secure Boot replaces application security or endpoint protection.
