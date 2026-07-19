# iptables Cheat Sheet

> **Applies to:** Linux systems using the iptables command interface
> **Last reviewed:** 2026-07-14

`iptables` is still widely used, but many current Linux distributions use the nftables framework underneath or prefer the native `nft` command. Confirm whether the host uses `iptables-legacy`, `iptables-nft`, firewalld, UFW, or native nftables before changing rules.

## Safety

> [!DANGER]
> A remote firewall change can immediately lock you out. Keep an existing privileged session open, save the current rules, schedule an automatic rollback when possible, and verify console or out-of-band access.

## Identify the firewall backend

```bash
iptables --version
update-alternatives --display iptables 2>/dev/null || true
nft list ruleset
systemctl status firewalld --no-pager
ufw status verbose
```

## Back up and inspect

```bash
sudo iptables-save > "iptables-backup-$(date +%Y%m%d-%H%M%S).rules"
sudo iptables -S
sudo iptables -L --numeric --verbose --line-numbers
sudo iptables -t nat -S
sudo iptables -t mangle -S
```

Use `-C` to check whether a rule already exists:

```bash
sudo iptables -C INPUT -p tcp --dport <port> -j ACCEPT
```

## Common stateful rules

Prefer `conntrack` rather than the older `state` match:

```bash
sudo iptables -A INPUT -m conntrack --ctstate INVALID -j DROP
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -i lo -j ACCEPT
```

Allow SSH from a management network:

```bash
sudo iptables -A INPUT \
  -p tcp \
  -s <management-cidr> \
  --dport 22 \
  -m conntrack --ctstate NEW \
  -j ACCEPT
```

Allow a TCP or UDP service:

```bash
sudo iptables -A INPUT -p tcp --dport <port> -j ACCEPT
sudo iptables -A INPUT -p udp --dport <port> -j ACCEPT
```

## Rule order

Rules are evaluated in order. Insert a rule at a specific position when required:

```bash
sudo iptables -I INPUT <line-number> <rule-specification>
```

Delete by exact rule specification when possible:

```bash
sudo iptables -D INPUT -p tcp --dport <port> -j ACCEPT
```

Or review numbered rules and then delete a line:

```bash
sudo iptables -L INPUT --numeric --line-numbers
sudo iptables -D INPUT <line-number>
```

Line numbers change after deletion, so list the chain again before deleting another rule.

## Default policies

Inspect current policies before changing them:

```bash
sudo iptables -S
```

Example policy change:

```bash
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT
```

> [!WARNING]
> Add required management, loopback, established-session, and service rules **before** setting a default DROP policy.

## Logging with rate limiting

```bash
sudo iptables -A INPUT \
  -m limit --limit 5/min --limit-burst 10 \
  -j LOG --log-prefix "iptables-input-drop: " --log-level warning
```

An unconditional LOG rule can flood system logs.

## NAT and forwarding

Enable IPv4 forwarding using persistent system configuration rather than a one-off command:

```bash
sysctl net.ipv4.ip_forward
```

Masquerade traffic leaving an interface:

```bash
sudo iptables -t nat -A POSTROUTING -o <egress-interface> -j MASQUERADE
```

Destination NAT:

```bash
sudo iptables -t nat -A PREROUTING \
  -i <ingress-interface> \
  -p tcp --dport <external-port> \
  -j DNAT --to-destination <destination-ip>:<destination-port>
```

Also permit the forwarded traffic in the `FORWARD` chain and validate the return path.

## Rate limiting

```bash
sudo iptables -A INPUT \
  -p tcp --dport <port> \
  -m conntrack --ctstate NEW \
  -m limit --limit <rate>/second --limit-burst <burst> \
  -j ACCEPT
```

## Misleading patterns to avoid

- An iptables destination using a DNS name is resolved when the rule is created; it does not continuously track DNS changes.
- Redirecting TCP port 80 to 443 does not add TLS. The application on the destination port must actually speak TLS.
- Dropping all ICMP can break Path MTU Discovery and troubleshooting. Filter specific types only with a documented reason.
- Flushing rules remotely without a tested rollback can disconnect the host.

## Persistence

Persistence is distribution-specific. On Debian-based systems using `iptables-persistent`:

```bash
sudo iptables-save | sudo tee /etc/iptables/rules.v4 >/dev/null
sudo ip6tables-save | sudo tee /etc/iptables/rules.v6 >/dev/null
```

Verify the persistence service and restore behavior before rebooting.

## Restore and rollback

Validate a saved ruleset in a lab when possible, then restore:

```bash
sudo iptables-restore < iptables-backup.rules
```

For remote maintenance, use `iptables-apply` when available because it can automatically roll back unconfirmed changes:

```bash
sudo iptables-apply
```

## Official references

- [Netfilter documentation](https://www.netfilter.org/documentation/)
- [iptables manual](https://man7.org/linux/man-pages/man8/iptables.8.html)
- [nftables wiki](https://wiki.nftables.org/)