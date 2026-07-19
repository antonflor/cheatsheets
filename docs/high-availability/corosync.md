# Corosync Quick Reference

> **Applies to:** Corosync 3.x concepts on modern Linux high-availability clusters
> **Last reviewed:** 2026-07-18

Corosync provides cluster membership, quorum, and group communication services. It is commonly paired with Pacemaker, which manages resources and service placement.

## Core concepts

| Concept | Purpose |
|---|---|
| Membership | Tracks which cluster nodes are currently participating |
| Quorum | Prevents unsafe cluster actions when sufficient votes are unavailable |
| Totem | Ordered group-communication protocol used by Corosync |
| KNET | Common transport layer for node-to-node communication |
| Votequorum | Quorum provider for node voting and expected-vote calculations |

## Read-only inspection

```bash
systemctl status corosync
corosync-cfgtool -s
corosync-quorumtool -s
corosync-cmapctl
journalctl -u corosync -b
```

When Pacemaker is present, also inspect the resource layer:

```bash
crm_mon -1Arf
pcs status
```

Use the command available for the cluster's management stack.

## Configuration locations

Common files and directories include:

```text
/etc/corosync/corosync.conf
/etc/corosync/authkey
/etc/corosync/uidgid.d/
```

Configuration normally defines node identities, transport addresses, quorum behavior, logging, and the cluster name.

> [!WARNING]
> Membership, quorum, vote, transport, and node-ID changes can split a cluster or cause resource fencing. Capture the current configuration and cluster state, verify fencing, and follow the platform's rolling-change procedure.

## Quorum checks

```bash
corosync-quorumtool -s
corosync-quorumtool -l
```

Confirm:

- expected votes;
- total votes;
- quorum threshold;
- current membership;
- whether the cluster is quorate;
- qdevice participation where used.

Do not casually configure a two-node cluster to ignore quorum. Pair the quorum design with functional fencing and application-specific failure testing.

## Network troubleshooting

1. Confirm node addressing and name resolution.
2. Check interface state, MTU, drops, and errors.
3. Verify required UDP traffic is permitted between every cluster node.
4. Compare `corosync.conf` node IDs and addresses on all nodes.
5. Review Corosync journal timestamps for membership changes and token timeouts.
6. Check clock synchronization and sustained latency or packet loss.
7. Confirm both cluster communication paths when redundant links are configured.

Useful commands:

```bash
ip -br address
ip -s link
ss -uanp
ping <peer-address>
journalctl -u corosync --since '<time>'
```

## Service control

```bash
systemctl start corosync
systemctl stop corosync
systemctl restart corosync
systemctl enable corosync
```

> [!CAUTION]
> Stopping or restarting Corosync changes cluster membership and can trigger fencing or resource movement. Put the node into the appropriate maintenance or standby state through the cluster manager first.

## Relationship to Pacemaker

Corosync provides communication and membership; Pacemaker makes resource-management decisions. See [pacemaker.md](pacemaker.md) for resource and constraint operations.

## References

- [Corosync documentation](https://corosync.github.io/corosync/)
