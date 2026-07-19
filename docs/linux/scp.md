# SCP Quick Reference

> **Applies to:** OpenSSH `scp` clients
> **Last reviewed:** 2026-07-18

`scp` copies files over SSH. Current OpenSSH releases use the SFTP protocol for normal `scp` transfers by default, while retaining familiar command-line syntax.

## Basic syntax

```text
scp [options] <source> <destination>
```

Local to remote:

```bash
scp <local-file> <user>@<host>:<remote-path>
```

Remote to local:

```bash
scp <user>@<host>:<remote-file> <local-path>
```

Copy a directory recursively:

```bash
scp -r <local-directory> <user>@<host>:<remote-directory>
```

## Common options

| Option | Purpose |
|---|---|
| `-P <port>` | Use a nondefault SSH port |
| `-i <identity-file>` | Use a specific SSH private key |
| `-r` | Copy directories recursively |
| `-p` | Preserve timestamps and file modes |
| `-C` | Enable SSH compression |
| `-l <kbit/s>` | Limit bandwidth |
| `-v` | Verbose SSH diagnostics |
| `-J <jump-host>` | Connect through an SSH jump host |
| `-O` | Request the legacy SCP protocol where supported and required |

Examples:

```bash
scp -P <port> -i <key> <file> <user>@<host>:<path>
scp -J <jump-user>@<jump-host> <file> <user>@<host>:<path>
scp -p <user>@<host>:<remote-file> <local-path>
```

## Safety guidance

> [!CAUTION]
> Confirm the source and destination carefully, especially when running as a privileged user or copying directories recursively. Existing destination files can be overwritten.

- Use host aliases and keys in `~/.ssh/config` to reduce repeated sensitive arguments.
- Verify the server host key rather than bypassing host-key checking.
- Protect private keys with appropriate filesystem permissions and a passphrase where practical.
- Use `rsync` over SSH for resumable synchronization, deletion previews, and large directory trees.
- Use `sftp` when an interactive file-transfer session is more appropriate.

## Troubleshooting

Verbose connection output:

```bash
scp -v <source> <destination>
```

Check the underlying SSH connection:

```bash
ssh -v <user>@<host>
```

Common checks:

- destination path and permissions;
- available disk space;
- SSH port and firewall policy;
- key selection and agent state;
- jump-host configuration;
- whether the server permits SFTP or requires legacy SCP behavior;
- filename quoting and shell expansion.

## References

- [OpenSSH scp manual](https://man.openbsd.org/scp)
