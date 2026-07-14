# Cisco IOS and IOS XE Cheat Sheet

> **Applies to:** Common Cisco IOS and IOS XE switching and routing workflows
> **Last reviewed:** 2026-07-14

Command availability and output vary by platform and release. This sheet intentionally excludes Arista EOS MLAG syntax and NX-OS-only commands.

## Safety

> [!WARNING]
> Debugging, clearing protocol sessions, changing trunk VLANs, and modifying spanning-tree settings can interrupt production traffic. Capture the current state and confirm a rollback method first.

## Device and system state

```text
show version
show inventory
show running-config
show startup-config
show logging
show clock
show processes cpu sorted
show processes memory sorted
show environment all
show redundancy
show boot
show file systems
show flash:
```

## Interfaces

```text
show interfaces status
show interfaces description
show ip interface brief
show interfaces <interface>
show interfaces <interface> switchport
show interfaces <interface> counters errors
show interfaces counters errors
show running-config interface <interface>
show controllers ethernet-controller <interface> phy
```

Cable diagnostics are platform-dependent:

```text
test cable-diagnostics tdr interface <interface>
show cable-diagnostics tdr interface <interface>
```

Clear counters only when you have recorded the previous values:

```text
clear counters <interface>
```

## VLANs and trunks

```text
show vlan brief
show interfaces trunk
show interfaces <interface> switchport
show spanning-tree vlan <vlan-id>
```

Example access port:

```text
interface <interface>
 description <description>
 switchport
 switchport mode access
 switchport access vlan <vlan-id>
 spanning-tree portfast
 spanning-tree bpduguard enable
```

Example trunk:

```text
interface <interface>
 description <description>
 switchport
 switchport mode trunk
 switchport trunk allowed vlan <vlan-list>
```

> [!CAUTION]
> `switchport trunk allowed vlan <list>` replaces the effective allowed list in many workflows. Use `add` or `remove` only when that is the intended change, and verify with `show interfaces trunk`.

## Spanning Tree Protocol

```text
show spanning-tree summary
show spanning-tree root
show spanning-tree vlan <vlan-id>
show spanning-tree interface <interface> detail
show spanning-tree inconsistentports
```

Avoid broad debugging in production. If debugging is required, constrain it and disable it immediately afterward:

```text
show debugging
undebug all
```

## EtherChannel and LACP

```text
show etherchannel summary
show etherchannel port-channel
show lacp neighbor
show lacp counters
show interfaces port-channel <number>
```

Example LACP bundle:

```text
interface range <member-interfaces>
 channel-group <number> mode active

interface port-channel <number>
 description <description>
```

## MAC, ARP, and neighbors

```text
show mac address-table
show mac address-table dynamic
show mac address-table interface <interface>
show arp
show ip arp
show cdp neighbors detail
show lldp neighbors detail
```

Clear dynamic MAC entries only for a confirmed troubleshooting need:

```text
clear mac address-table dynamic interface <interface>
```

## Layer 3 and routing

```text
show ip route
show ip route <prefix>
show ip protocols
show ip cef <prefix> detail
show ip interface <interface>
show access-lists
show ip access-lists
show vrf
show ip route vrf <vrf-name>
```

## First-hop redundancy

```text
show standby brief
show standby
show vrrp brief
```

## Power over Ethernet

```text
show power inline
show power inline <interface> detail
```

## Configuration handling

```text
show archive config differences nvram:startup-config system:running-config
copy running-config startup-config
```

Before a risky remote change, use a rollback-capable workflow supported by the platform, such as configuration archive and `reload in`, and confirm the exact behavior in the device documentation.

## Troubleshooting workflow

1. Confirm scope: one endpoint, one VLAN, one interface, or a wider failure.
2. Check interface state and counters.
3. Validate VLAN, trunk, and spanning-tree state.
4. Confirm MAC learning, ARP, and neighbor discovery.
5. Check routing and access-control policy.
6. Review logs with synchronized timestamps.
7. Make the smallest reversible change and verify recovery.

## Official references

- [Cisco IOS and NX-OS software documentation](https://www.cisco.com/c/en/us/support/ios-nx-os-software/index.html)
- [Cisco IOS XE configuration guides](https://www.cisco.com/c/en/us/support/ios-nx-os-software/ios-xe/index.html)