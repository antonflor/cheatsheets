# Link Layer Discovery Protocol

> **Applies to:** IEEE 802.1AB LLDP concepts with common Cisco IOS/IOS XE, Arista EOS, and Junos examples
> **Last reviewed:** 2026-07-18

Link Layer Discovery Protocol (LLDP) lets directly connected devices advertise identity, port, management, and capability information to each other. It is useful for topology discovery, cabling validation, inventory, voice endpoint provisioning, and troubleshooting.

## What LLDP exchanges

LLDP information is encoded as Type-Length-Value elements, commonly including:

- chassis ID;
- port ID and port description;
- system name and description;
- system capabilities;
- management address;
- optional LLDP-MED information for voice and media endpoints.

LLDP is link-local. Advertisements are not routed beyond the directly connected segment.

## Read-only inspection

### Cisco IOS and IOS XE

```text
show lldp
show lldp neighbors
show lldp neighbors detail
show lldp interface <interface>
show lldp traffic
```

### Arista EOS

```text
show lldp
show lldp neighbors
show lldp neighbors detail
```

### Junos

```text
show lldp
show lldp neighbors
show lldp neighbors detail
show lldp statistics
```

## Common configuration examples

> [!CAUTION]
> LLDP syntax and defaults vary by vendor and release. Confirm the exact platform behavior before changing a production interface.

Cisco IOS or IOS XE global enablement:

```text
lldp run
```

Cisco interface controls:

```text
interface <interface>
 lldp transmit
 lldp receive
```

Junos enablement:

```text
set protocols lldp interface all
```

Junos per-interface enablement:

```text
set protocols lldp interface <interface>
```

## Troubleshooting workflow

1. Confirm the physical interface is up and error-free.
2. Verify LLDP is enabled globally and on both interface directions.
3. Check whether advertisements and receives are incrementing.
4. Compare the observed neighbor, port, VLAN, and management address against the intended cabling map.
5. Confirm intermediate devices, media converters, or hypervisors are not hiding or rewriting discovery information.
6. Check for LLDP-MED requirements on phones and other media endpoints.

## Security guidance

LLDP can disclose hostnames, platform details, management addresses, and topology information. Disable transmission where disclosure is not appropriate, such as untrusted external handoffs, while considering operational and voice-device requirements.

Do not assume LLDP should always be disabled on ordinary endpoint-facing ports. Phones, hypervisors, access points, automation systems, and inventory tooling may rely on it. Apply policy according to the actual endpoint and trust boundary.

## References

- [IEEE 802.1AB overview](https://1.ieee802.org/maintenance/802-1ab/)
