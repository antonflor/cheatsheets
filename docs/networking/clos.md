# Clos Network Design

> **Applies to:** General multistage Clos and folded-Clos network concepts
> **Last reviewed:** 2026-07-18

A Clos network uses multiple switching stages and parallel paths to scale bandwidth and reduce the impact of individual link or switch failures. Modern leaf-spine data center fabrics are commonly implemented as folded-Clos topologies.

## Core structure

| Stage | Purpose |
|---|---|
| Ingress or leaf | Connects endpoints, racks, services, or lower-stage switches |
| Middle or spine | Provides parallel transit paths between edge stages |
| Egress or leaf | Connects the destination endpoints or downstream stage |

In a folded-Clos fabric, the ingress and egress stages are represented by the same leaf tier.

## Design properties

- Multiple equal-cost paths can exist between leaves.
- Capacity scales horizontally by adding links, leaves, or spines within platform limits.
- A spine failure should normally reduce available capacity rather than disconnect the fabric.
- Oversubscription depends on the ratio of endpoint-facing bandwidth to fabric-facing bandwidth.
- “Non-blocking” is a capacity-design target, not an automatic property of every Clos deployment.

## Basic capacity model

For one leaf switch:

```text
downlink capacity = sum of endpoint-facing bandwidth
uplink capacity   = sum of leaf-to-spine bandwidth
oversubscription  = downlink capacity / uplink capacity
```

Example:

```text
48 x 25 Gb/s downlinks = 1.2 Tb/s
8 x 100 Gb/s uplinks   = 0.8 Tb/s
oversubscription       = 1.5:1
```

## Operational considerations

- Connect every participating leaf to every spine in the same fabric tier when the design assumes full-path diversity.
- Validate ECMP width, routing-table scale, forwarding-table scale, optics, cabling, power, and cooling.
- Monitor link utilization, errors, drops, routing adjacencies, and the number of installed next hops.
- Test full failures and partial failures such as packet loss, MTU mismatch, optic degradation, and unidirectional links.
- Use a source of truth for addressing, ASNs, interface assignments, and cabling.

## Relationship to leaf-spine

A leaf-spine fabric is a common folded-Clos implementation. See [leaf-spine.md](leaf-spine.md) for routing, overlays, multihoming, and troubleshooting guidance.

## References

- [RFC 7938: Use of BGP for Routing in Large-Scale Data Centers](https://www.rfc-editor.org/rfc/rfc7938)
