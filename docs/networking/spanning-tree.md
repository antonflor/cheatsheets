# Spanning Tree Protocol Cheat Sheet

> **Applies to:** IEEE STP/RSTP/MSTP concepts with Cisco IOS and IOS XE examples
> **Last reviewed:** 2026-07-14

A practical reference for understanding Layer 2 loop prevention, identifying the root bridge, interpreting port roles and states, and troubleshooting common spanning-tree failures.

> [!WARNING]
> Spanning-tree changes can move the root bridge, reconverge many VLANs, or create a Layer 2 loop. Capture the current topology and verify the intended root, alternate paths, PortFast scope, and rollback plan before changing production switches.

## Why spanning tree exists

Redundant Layer 2 links are useful for availability, but Ethernet frames do not contain a hop limit. An active Layer 2 loop can cause broadcast storms, duplicate frames, MAC-address-table instability, and severe control-plane load.

Spanning Tree Protocol (STP) exchanges Bridge Protocol Data Units (BPDUs), elects a root bridge, calculates a loop-free tree, and places redundant paths into a nonforwarding role until they are needed.

## Common variants

| Variant | Purpose |
|---|---|
| STP | Original slow-converging spanning tree behavior |
| RSTP | Rapid convergence using alternate and backup port roles |
| PVST+ | Cisco per-VLAN spanning tree implementation |
| Rapid PVST+ | Cisco rapid per-VLAN implementation |
| MSTP | Maps many VLANs into a smaller number of spanning-tree instances |

Do not assume every vendor implements the same defaults or per-VLAN behavior. Verify the operating mode and interoperability requirements on every switch family.

## Election logic

### Root bridge

The switch with the lowest Bridge ID becomes the root bridge.

Bridge ID selection is based on:

1. lowest bridge priority;
2. lowest system ID extension or VLAN identifier where applicable;
3. lowest switch MAC address.

A lower numerical value wins.

### Root port

Each nonroot switch selects one root port: the port with the best path toward the root bridge.

Selection commonly considers:

1. lowest root path cost;
2. lowest upstream bridge ID;
3. lowest upstream port priority;
4. lowest upstream port number.

### Designated port

Each Layer 2 segment selects one designated port that provides the best path toward the root. Other redundant ports become alternate, backup, or blocking/discarding depending on the mode.

## Port roles

| Role | Meaning |
|---|---|
| Root | Best path from a nonroot switch to the root bridge |
| Designated | Forwarding port selected for a segment |
| Alternate | Backup path to the root in RSTP/MSTP |
| Backup | Backup path to the same shared segment |
| Disabled | Administratively or operationally unavailable |

## Port states

### Classic STP

| State | Behavior |
|---|---|
| Blocking | Receives BPDUs but does not forward user traffic |
| Listening | Participates in topology calculation |
| Learning | Learns source MAC addresses but does not forward user traffic |
| Forwarding | Learns and forwards traffic |
| Disabled | Does not participate |

### RSTP and MSTP

| State | Behavior |
|---|---|
| Discarding | Combines classic blocking, listening, and disabled behavior |
| Learning | Learns source MAC addresses |
| Forwarding | Learns and forwards traffic |

## Inspect the current topology

```text
show spanning-tree
show spanning-tree summary
show spanning-tree root
show spanning-tree vlan <vlan-id>
show spanning-tree vlan <vlan-id> detail
show spanning-tree interface <interface> detail
show interfaces <interface> status
show interfaces <interface> counters errors
show mac address-table dynamic
show logging
```

Questions to answer before changing anything:

- Which switch is root for each VLAN or MST instance?
- Is the root located where the design expects it?
- Which ports are root, designated, and alternate?
- Are BPDUs increasing on the expected links?
- Are blocked links actually receiving BPDUs?
- Are speed, duplex, VLAN, and trunk settings consistent on both ends?
- Did the topology recently reconverge?

## Select the root bridge

Inspect first:

```text
show spanning-tree root
show spanning-tree vlan <vlan-id>
```

Cisco root-primary and root-secondary macros:

```text
spanning-tree vlan <vlan-list> root primary
spanning-tree vlan <vlan-list> root secondary
```

Set priority explicitly when deterministic values are required:

```text
spanning-tree vlan <vlan-list> priority <priority>
```

Bridge priority is normally configured in increments supported by the platform, commonly increments of 4096.

> [!CAUTION]
> Do not allow access-layer switches to win root election by accident. Place the primary and secondary root near the Layer 3 gateway or according to the intended traffic path.

## Port cost and port priority

Inspect the current decision:

```text
show spanning-tree vlan <vlan-id>
show spanning-tree interface <interface> detail
```

Influence path selection only when the physical design and failure behavior are understood:

```text
interface <interface>
 spanning-tree vlan <vlan-list> cost <cost>
 spanning-tree vlan <vlan-list> port-priority <priority>
```

Prefer root placement and sound topology design over aggressive timer tuning.

## PortFast and edge ports

PortFast allows an endpoint-facing port to move rapidly to forwarding instead of waiting through normal convergence stages.

```text
interface <interface>
 spanning-tree portfast
```

For a trunk connected only to a trusted endpoint such as a hypervisor or server:

```text
interface <interface>
 spanning-tree portfast trunk
```

> [!DANGER]
> Do not enable PortFast on links to switches, bridges, hubs, or unknown downstream devices. A redundant connection can transition immediately to forwarding and create a loop.

## BPDU Guard

BPDU Guard protects edge ports by placing a port into an error-disabled state if it receives a BPDU.

Global default for PortFast-enabled ports:

```text
spanning-tree portfast edge bpduguard default
```

Interface configuration on platforms using classic syntax:

```text
interface <interface>
 spanning-tree bpduguard enable
```

Verify:

```text
show spanning-tree summary
show errdisable recovery
show interfaces status err-disabled
show logging
```

Investigate the downstream device and cabling before recovering the port.

## Root Guard

Root Guard prevents a downstream-facing port from accepting superior BPDUs and becoming the path to a new root bridge.

```text
interface <interface>
 spanning-tree guard root
```

Use Root Guard where a downstream switch must never become root. Do not use it on a port that may legitimately become a root port after a failure.

## Loop Guard

Loop Guard helps prevent a non-designated port from moving to forwarding when BPDUs unexpectedly stop arriving.

```text
interface <interface>
 spanning-tree guard loop
```

Loop Guard is commonly useful on point-to-point switch links. Validate platform interaction with Unidirectional Link Detection (UDLD), EtherChannel, and the selected spanning-tree mode.

## BPDU Filter

BPDU Filter suppresses BPDU transmission or processing depending on how and where it is configured.

> [!DANGER]
> BPDU Filter can effectively remove spanning-tree protection and create silent loops. Avoid interface-level BPDU filtering unless the design specifically requires it and the consequences are fully understood.

## Rapid PVST+ example

Inspect current mode:

```text
show spanning-tree summary
```

Configure Rapid PVST+:

```text
spanning-tree mode rapid-pvst
```

Verify every participating switch supports the intended mode and interoperability behavior before changing a production domain.

## MSTP concepts

Multiple Spanning Tree Protocol maps VLANs to instances. Switches are in the same MST region only when their configuration identity matches, including:

- region name;
- revision number;
- VLAN-to-instance mapping.

Example:

```text
spanning-tree mode mst
spanning-tree mst configuration
 name <region-name>
 revision <revision>
 instance <instance-id> vlan <vlan-list>
 exit
```

Verify:

```text
show spanning-tree mst configuration
show spanning-tree mst
show spanning-tree mst <instance-id>
```

A mapping mismatch creates an MST region boundary even when the physical switches are adjacent.

## EtherChannel interaction

Spanning tree should treat a correctly formed EtherChannel as one logical link.

```text
show etherchannel summary
show interfaces port-channel <number>
show spanning-tree interface port-channel <number> detail
```

Member links must have consistent trunking, native VLAN, allowed VLANs, speed, duplex, and channel configuration. A partially formed channel can produce unexpected spanning-tree behavior.

## Troubleshooting workflow

### 1. Confirm the blast radius

```text
show logging
show spanning-tree detail
show processes cpu
show interfaces counters errors
```

Look for topology changes, MAC flapping, high CPU, broadcast growth, interface errors, and repeated link transitions.

### 2. Identify the root and blocked paths

```text
show spanning-tree root
show spanning-tree vlan <vlan-id>
```

Compare actual roles against the intended diagram.

### 3. Verify BPDU flow

```text
show spanning-tree vlan <vlan-id> detail
show spanning-tree interface <interface> detail
```

Run the detail command more than once and confirm BPDU counters increase on root, designated, and blocked/alternate links where expected.

### 4. Check the physical link

```text
show interfaces <interface>
show interfaces <interface> status
show interfaces <interface> counters errors
```

Investigate duplex mismatch, CRC errors, drops, overloaded links, unidirectional failure, optics, cabling, and transceiver alarms.

### 5. Check Layer 2 consistency

```text
show interfaces trunk
show vlan brief
show etherchannel summary
show running-config interface <interface>
```

Confirm both ends agree on trunking, native VLAN, allowed VLANs, channel membership, and spanning-tree protections.

### 6. Examine topology changes

```text
show spanning-tree detail
show logging | include SPANTREE|LINK|LINEPROTO
```

Identify the interface generating repeated topology changes rather than treating the root switch as the source of every event.

### 7. Debug only when necessary

```text
debug spanning-tree events
debug spanning-tree bpdu
```

> [!CAUTION]
> Debug commands can increase control-plane load and generate substantial output. Use logging buffers, limit the duration, and disable debugging after collecting evidence.

## Common failure patterns

| Symptom | Likely checks |
|---|---|
| Unexpected root bridge | Bridge priority, new switch, superior BPDUs, Root Guard |
| Port repeatedly changes state | Link flap, channel mismatch, unstable neighbor, physical errors |
| Blocked port moves to forwarding | Lost BPDUs, unidirectional link, duplex mismatch, Loop Guard |
| Access port error-disabled | BPDU Guard, downstream switch, accidental patch loop |
| High CPU and MAC flapping | Active Layer 2 loop, EtherChannel inconsistency, lost BPDUs |
| MST boundary appears | Region name, revision, VLAN-to-instance mapping |
| Slow endpoint connectivity | Missing PortFast on a true endpoint port, authentication timing |

## Design guidance

- Choose the root and secondary root intentionally.
- Keep the root close to the expected Layer 3 gateway and traffic path.
- Use routed links instead of large Layer 2 failure domains where practical.
- Use PortFast only for verified endpoints and pair it with BPDU Guard.
- Use Root Guard at boundaries where downstream switches must not become root.
- Use Loop Guard or UDLD where loss of BPDUs or unidirectional links is a concern.
- Avoid unnecessary spanning-tree timer changes.
- Keep VLAN and MST instance scope small enough to troubleshoot.
- Document expected root ports and alternate paths.
- Test link, switch, and port-channel failures before production rollout.

## References

- [Cisco: Understand and Configure STP on Catalyst Switches](https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol/5234-5.html)
- [Cisco: Troubleshoot STP Problems and Design Considerations](https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol/10556-16.html)
- [Cisco: Understand Rapid Spanning Tree Protocol](https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol/24062-146.html)
- [Cisco: Understand Multiple Spanning Tree Protocol](https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol/24248-147.html)
