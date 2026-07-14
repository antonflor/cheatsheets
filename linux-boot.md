# Linux Boot and Kernel Cheat Sheet

> **Applies to:** Modern Linux systems using systemd, GRUB, and common distribution tooling
> **Last reviewed:** 2026-07-14

A practical reference for the Linux boot sequence, kernel command line, initramfs, systemd targets, boot troubleshooting, modules, and recovery.

> [!WARNING]
> Bootloader, kernel, initramfs, filesystem, and module changes can leave a system unbootable. Preserve a known-good kernel, console access, backups, and a tested recovery path before making changes.

## Boot sequence

1. **Firmware:** BIOS or UEFI initializes hardware and selects a boot entry.
2. **Bootloader:** GRUB or another bootloader loads the kernel and initramfs.
3. **Kernel:** The kernel initializes CPU, memory, drivers, and core subsystems.
4. **Initramfs:** Early userspace discovers storage, unlocks encryption, assembles RAID or LVM, and mounts the real root filesystem.
5. **PID 1:** The kernel starts the init process, commonly systemd.
6. **Userspace:** systemd mounts filesystems, starts services, and reaches the configured target.

## Identify the running system

```bash
uname -a
uname -r
cat /etc/os-release
cat /proc/cmdline
systemd-detect-virt
systemctl get-default
systemctl is-system-running
```

## Kernel and boot files

```bash
ls -lh /boot
ls -lh /boot/efi
find /boot -maxdepth 1 -type f -printf '%f\n'
```

Common files:

| File | Purpose |
|---|---|
| `vmlinuz-*` | Compressed Linux kernel image |
| `initrd.img-*` or `initramfs-*` | Early userspace image |
| `config-*` | Kernel build configuration |
| `System.map-*` | Kernel symbol map |
| `/boot/grub/grub.cfg` | Generated GRUB configuration |
| `/etc/default/grub` | Distribution-level GRUB defaults |
| `/etc/fstab` | Filesystems and mount behavior |

Do not edit generated `grub.cfg` files directly unless the platform explicitly requires it. Change the source configuration and regenerate the file.

## Kernel command line

Show current parameters:

```bash
cat /proc/cmdline
```

Common temporary troubleshooting parameters:

| Parameter | Effect |
|---|---|
| `systemd.unit=rescue.target` | Boot into rescue mode |
| `systemd.unit=emergency.target` | Boot into minimal emergency mode |
| `rd.break` | Break into initramfs on supported distributions |
| `nomodeset` | Disable normal graphics mode setting for troubleshooting |
| `single` or `1` | Request a single-user or rescue-like boot on some systems |
| `init=/bin/bash` | Start a shell as PID 1; use only for controlled recovery |

> [!CAUTION]
> Kernel parameters differ by distribution, bootloader, initramfs implementation, and security configuration. Temporary console edits are safer than permanent changes while diagnosing an issue.

## systemd boot targets

```bash
systemctl list-units --type=target
systemctl get-default
systemctl set-default multi-user.target
systemctl set-default graphical.target
systemctl isolate rescue.target
systemctl isolate emergency.target
```

> [!WARNING]
> `systemctl isolate` stops units that are not required by the target and can disconnect remote sessions. Use console access for rescue operations.

## Analyze boot performance

```bash
systemd-analyze
systemd-analyze blame
systemd-analyze critical-chain
systemd-analyze plot > boot.svg
```

`blame` shows activation time, not necessarily the root cause. Use `critical-chain` and logs to understand ordering and dependency delays.

## Inspect boot logs

Current boot:

```bash
journalctl -b
journalctl -b -p warning
journalctl -b -u <unit>
dmesg --human
```

Previous boot:

```bash
journalctl -b -1
journalctl -b -1 -p warning
```

List recorded boots:

```bash
journalctl --list-boots
```

If previous boots are unavailable, persistent journal storage may not be enabled.

## Service failures during boot

```bash
systemctl --failed
systemctl status <unit>
journalctl -b -u <unit>
systemctl show <unit> -p After -p Before -p Requires -p Wants
systemctl list-dependencies <unit>
```

Reset a unit's failed state only after collecting evidence:

```bash
systemctl reset-failed <unit>
```

## Filesystem and mount failures

```bash
findmnt
findmnt --verify
lsblk -f
blkid
cat /etc/fstab
systemctl --failed --type=mount
journalctl -b | grep -iE 'mount|filesystem|fsck'
```

Test an `fstab` change without rebooting:

```bash
sudo mount -av
```

> [!CAUTION]
> `mount -a` can still affect live mount points. Review the exact `fstab` change and use maintenance controls for production systems.

For noncritical network or removable mounts, options such as `nofail`, `_netdev`, or systemd automount behavior may prevent unnecessary boot failure, but they change availability semantics.

## Initramfs inspection and rebuild

Debian and Ubuntu:

```bash
lsinitramfs /boot/initrd.img-$(uname -r) | less
sudo update-initramfs -u -k $(uname -r)
```

RHEL, Fedora, Rocky, and related systems:

```bash
lsinitrd /boot/initramfs-$(uname -r).img | less
sudo dracut --force /boot/initramfs-$(uname -r).img $(uname -r)
```

Rebuild an initramfs when required storage, encryption, filesystem, or early-boot drivers are missing.

> [!WARNING]
> Confirm the target kernel version and verify free space in `/boot` before rebuilding. Do not overwrite the only known-good boot image without a recovery option.

## GRUB configuration

Inspect defaults:

```bash
cat /etc/default/grub
```

Debian and Ubuntu:

```bash
sudo update-grub
```

Common RHEL-family BIOS path:

```bash
sudo grub2-mkconfig -o /boot/grub2/grub.cfg
```

Common RHEL-family UEFI path varies by distribution and version. Determine the supported generated configuration path before running `grub2-mkconfig`.

List firmware boot entries:

```bash
sudo efibootmgr -v
```

## Kernel packages

Debian and Ubuntu:

```bash
dpkg -l 'linux-image*' | grep '^ii'
apt list --installed 'linux-image*'
```

RHEL-family systems:

```bash
rpm -q kernel
sudo grubby --default-kernel
sudo grubby --info=ALL
```

Keep at least one known-good previous kernel until the new kernel has booted and passed validation.

## Kernel modules

```bash
lsmod
modinfo <module>
sudo modprobe <module>
sudo modprobe -r <module>
journalctl -k -b
dmesg --human | grep -i <module>
```

Persistent module loading commonly uses files under:

```text
/etc/modules-load.d/
```

Module options commonly use:

```text
/etc/modprobe.d/
```

> [!CAUTION]
> Removing storage, network, filesystem, or security modules can interrupt the running system. Inspect module dependencies with `modinfo` and test in a nonproduction environment.

## sysctl runtime settings

```bash
sysctl -a
sysctl <key>
sudo sysctl -w <key>=<value>
sudo sysctl --system
```

Persistent settings are commonly stored under:

```text
/etc/sysctl.conf
/etc/sysctl.d/*.conf
```

Capture the old value and understand namespace or container interactions before changing kernel parameters.

## Kernel and hardware information

```bash
lscpu
lsmem
lsblk
lspci -k
lsusb
cat /proc/meminfo
cat /proc/interrupts
cat /proc/modules
```

## Common boot failure patterns

| Symptom | Checks |
|---|---|
| Kernel panic: unable to mount root | Root device parameter, initramfs drivers, storage discovery, filesystem support |
| Emergency mode after mount failure | `/etc/fstab`, UUIDs, filesystem state, network mount dependencies |
| Boot hangs waiting for a device | Missing disk, incorrect UUID, timeout behavior, encrypted-volume or LVM activation |
| New kernel does not boot | Select previous kernel, compare initramfs and modules, inspect console logs |
| Network unavailable after boot | Driver or firmware, predictable interface names, NetworkManager/systemd-networkd status |
| Service delays boot | `systemd-analyze critical-chain`, unit dependencies, DNS, network-online target |
| UEFI cannot find bootloader | EFI System Partition, NVRAM entries, Secure Boot, bootloader installation |

## Recovery workflow

1. Photograph or capture the console error.
2. Try a known-good previous kernel from the bootloader.
3. Temporarily remove quiet or splash parameters to reveal messages.
4. Boot into rescue or emergency mode.
5. Confirm root filesystem, `/boot`, and EFI mounts.
6. Review the current and previous boot journal.
7. Validate `fstab`, kernel command line, and initramfs contents.
8. Rebuild only the affected artifact.
9. Regenerate the bootloader configuration when required.
10. Reboot with console access and validate services, storage, networking, and monitoring.

## Security controls affecting boot

- UEFI Secure Boot can reject unsigned kernels or modules.
- Kernel lockdown can restrict low-level access when Secure Boot is active.
- SELinux and AppArmor can block userspace actions after the kernel boots.
- LUKS encryption may require console input, TPM integration, or network-bound disk encryption.
- Measured boot and TPM policies can change after firmware, bootloader, or kernel updates.

Do not disable a security control merely to make a system boot without first understanding the failure and the resulting exposure.

## References

- [Linux kernel documentation](https://docs.kernel.org/)
- [systemd documentation](https://systemd.io/)
- [systemd-analyze manual](https://www.freedesktop.org/software/systemd/man/latest/systemd-analyze.html)
- [GNU GRUB manual](https://www.gnu.org/software/grub/manual/grub/)
