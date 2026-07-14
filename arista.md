# Arista EOS Cheat Sheet

> **Applies to:** Common Arista EOS data-center switching workflows
> **Last reviewed:** 2026-07-14

Command availability varies by EOS release and hardware platform. This sheet uses Arista terminology such as port-channel and MLAG rather than Cisco EtherChannel commands.

## Safety

> [!WARNING]
> Clearing routing sessions, changing MLAG peer links, modifying trunks, or enabling debug output can interrupt production traffic. Capture state and validate a rollback plan first.

## System state

```text
show version
show inventory
show hostname
show running-config
show startup-config
show logging last 100
show clock
show processes top once
show environment all
show reload cause
show extensions
```

## Interfaces

```text
show interfaces status
show interfaces description
show ip interface brief
show interfaces <interface>
show interfaces <interface> counters errors
show interfaces counters errors
show interfaces counters rates
show running-config interfaces <interface>
show transceiver <interface> detail
```

## VLANs and trunks

```text
show vlan
show vlan <vlan-id>
show interfaces trunk
show interfaces <interface> switchport
show spanning-tree
show spanning-tree vlan <vlan-id>
show spanning-tree inconsistentports
```

Example access port:

```text
interface <interface>
   description <description>
   switchport mode access
   switchport access vlan <vlan-id>
   spanning-tree portfast
```

Example trunk:

```text
interface <interface>
   description <description>
   switchport mode trunk
   switchport trunk allowed vlan <vlan-list>
```

## Port-channels and LACP

```text
show port-channel summary
show port-channel dense
show lacp neighbor
show lacp counters all-ports
show interfaces Port-Channel<number>
```

Example LACP member:

```text
interface <interface>
   channel-group <number> mode active
```

## MLAG

```text
show mlag
show mlag detail
show mlag interfaces
show mlag config-sanity
show mlag issu warnings
show interfaces Port-Channel<peer-link-number>
show interfaces Vlan<peer-vlan-id>
```

Check these before changing MLAG:

- peer-link state and member consistency;
- peer-address reachability;
- reload-delay and dual-primary settings;
- VLAN consistency across peers;
- orphaned ports and single-homed dependencies.

## BGP

```text
show ip bgp summary
show bgp ipv4 unicast summary
show ip bgp neighbors <neighbor-address>
show ip bgp <prefix>
show ip bgp neighbors <neighbor-address> advertised-routes
show ip bgp neighbors <neighbor-address> received-routes
show ip route bgp
show running-config section router bgp
```

Prefer a soft inbound or outbound refresh when policy permits rather than resetting the TCP session:

```text
clear ip bgp <neighbor-address> soft in
clear ip bgp <neighbor-address> soft out
```

## OSPF

```text
show ip ospf
show ip ospf neighbor
show ip ospf interface
show ip ospf database
show ip route ospf
```

## Routing, VRFs, MAC, and ARP

```text
show ip route
show ip route <prefix>
show vrf
show ip route vrf <vrf-name>
show mac address-table
show mac address-table interface <interface>
show ip arp
show ip arp vrf <vrf-name>
show lldp neighbors detail
```

## Configuration sessions and rollback

Use a configuration session for changes that benefit from an explicit diff:

```text
configure session <session-name>
show session-config diffs
commit
```

Save a validated running configuration:

```text
copy running-config startup-config
```

Confirm the exact session and rollback behavior for the deployed EOS release before relying on it during remote maintenance.

## Troubleshooting workflow

1. Verify system health and timestamps.
2. Check interface state, optics, errors, and rates.
3. Validate VLAN, trunk, spanning-tree, and port-channel consistency.
4. For dual-homed services, check MLAG state and config sanity.
5. Confirm MAC, ARP, VRF, and route resolution.
6. Inspect routing-protocol neighbors and policy.
7. Make the smallest reversible change and verify both peers afterward.

## Official references

- [Arista EOS documentation](https://www.arista.com/en/support/product-documentation)
- [Arista EOS Central](https://www.arista.com/en/support/toi/eos)