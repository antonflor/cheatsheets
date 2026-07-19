# Leaf-Spine Network Architecture Cheat Sheet

> **Applies to:** General routed data center fabric concepts
> **Last reviewed:** 2026-07-14

A practical reference for leaf-spine topology, capacity planning, routing, failure behavior, oversubscription, and common design choices.

## Topology

A leaf-spine fabric is a two-tier Clos topology:

- **Leaf switches** connect servers, storage, appliances, or access networks.
- **Spine switches** provide transit between leaf switches.
- Every leaf normally connects to every spine.
- Spines normally connect only to leaves in the same fabric tier.

Traffic between endpoints on different leaves follows a predictable path:

```text
endpoint -> leaf -> spine -> leaf -> endpoint
```

## Why use leaf-spine

- predictable hop count and latency;
- multiple equal-cost paths;
- horizontal scale by adding leaves or spines;
- failure of one spine usually reduces capacity rather than disconnecting the fabric;
- strong fit for east-west application traffic;
- automation-friendly, repeatable topology.

## Routed underlay

Modern fabrics commonly use Layer 3 point-to-point links between leaf and spine switches.

Typical choices:

- eBGP with private autonomous system numbers;
- OSPF or IS-IS in smaller or operator-specific designs;
- IPv4 or IPv6 numbered links;
- unnumbered links where platform support and operations are mature;
- Bidirectional Forwarding Detection (BFD) for faster failure detection when justified.

A routed underlay removes spanning tree from the leaf-to-spine fabric. Layer 2 loop prevention may still be required on server-facing bonds, multichassis links, legacy VLAN domains, or attached access networks.

## Equal-Cost Multipath

Equal-Cost Multipath (ECMP) distributes flows across available spine paths.

Verify:

- the routing table installs the expected number of next hops;
- hardware forwarding tables support the required ECMP width;
- the hash includes appropriate Layer 3 and Layer 4 fields;
- resilient hashing behavior meets failure and maintenance requirements;
- large flows do not create unacceptable polarization.

Example operational checks vary by vendor:

```text
show ip route <prefix>
show bgp <address-family> <prefix>
show forwarding route <prefix>
show interfaces counters rate
```

## Capacity and oversubscription

For one leaf:

```text
downlink capacity = sum of endpoint-facing bandwidth
uplink capacity = sum of leaf-to-spine bandwidth
oversubscription ratio = downlink capacity / uplink capacity
```

Example:

```text
48 x 25 Gb/s downlinks = 1.2 Tb/s
8 x 100 Gb/s uplinks   = 0.8 Tb/s
oversubscription       = 1.5:1
```

Consider real traffic patterns, not only port speed. Storage, backup, replication, AI, virtualization, and east-west service traffic can produce very different requirements.

## Scaling limits

The number of leaves supported by a spine tier is limited primarily by:

- available spine ports;
- required link speed;
- optics and cabling design;
- routing and forwarding table capacity;
- ECMP width;
- power and cooling;
- failure-domain policy.

Adding a spine increases available uplink capacity only when every participating leaf can connect to it.

## BGP underlay pattern

A common eBGP design uses:

- one autonomous system number per leaf;
- one shared or unique autonomous system number strategy for spines;
- point-to-point peerings between every leaf and spine;
- loopbacks advertised through the underlay;
- maximum-path configuration for ECMP;
- strict prefix policy and session authentication where supported.

Operational checks:

```text
show bgp summary
show bgp neighbors
show ip route <loopback-prefix>
show bfd peers
```

Avoid accepting or advertising unrestricted prefixes in the fabric underlay.

## Overlay options

A routed underlay can support an overlay such as VXLAN with BGP EVPN.

Common overlay functions:

- Layer 2 segments across leaves;
- distributed anycast gateways;
- tenant VRFs;
- MAC and IP advertisement;
- host mobility;
- integrated Layer 2 and Layer 3 services.

Keep underlay and overlay troubleshooting separate:

1. confirm physical links;
2. confirm underlay routing and loopback reachability;
3. confirm tunnel endpoint reachability;
4. confirm EVPN control-plane routes;
5. confirm VXLAN or encapsulation state;
6. confirm MAC, ARP/ND, and VRF forwarding.

## Server multihoming

Common choices include:

- active/standby bonds to one or two leaves;
- Link Aggregation Control Protocol (LACP) to one switch;
- multichassis LAG or MLAG to a leaf pair;
- EVPN multihoming;
- host routing with one or more routed interfaces.

Multichassis designs introduce state synchronization, split-brain handling, peer links, and failure cases that must be tested explicitly.

## Border and service leaves

Specialized leaf roles may include:

- border leaves for WAN, internet, cloud, or inter-data-center connectivity;
- service leaves for firewalls, load balancers, and appliances;
- storage leaves;
- edge leaves for legacy Layer 2 domains;
- superspine connectivity for multi-pod or larger Clos designs.

Do not create unnecessary hairpin paths through a distant service leaf without understanding bandwidth and failure impact.

## Failure behavior

| Failure | Expected effect in a healthy design |
|---|---|
| One leaf-to-spine link | Reduced path count for one leaf |
| One spine | Reduced fabric capacity, connectivity retained |
| One leaf | Endpoints on that leaf lose connectivity unless multihomed |
| One server link | Bonding, routing, or application redundancy handles failure |
| Control-plane session | ECMP path removed after protocol detection and convergence |
| Optic degradation | Errors or drops may occur before a hard link failure |

Test both hard failures and partial failures such as packet loss, unidirectional links, MTU mismatch, and high error rates.

## Maximum Transmission Unit

The underlay must support the overlay packet plus encapsulation overhead.

Verify end to end:

```text
show interfaces <interface>
ping <destination> size <size> do-not-fragment
```

Syntax varies by platform. Validate the maximum supported packet across host, leaf, spine, border, and appliance paths.

## Cabling and addressing

Recommended practices:

- use deterministic port maps;
- label both ends of every cable;
- reserve consistent leaf and spine interface ranges;
- generate point-to-point addressing from source-of-truth data;
- use loopbacks for stable router IDs and tunnel endpoints;
- record optic type, serial number, and expected distance;
- validate polarity and lane mapping for parallel optics.

## Monitoring

Track at minimum:

- interface state, errors, discards, and utilization;
- optic power and temperature;
- routing adjacency state and flap count;
- BFD state;
- ECMP next-hop count;
- control-plane CPU and memory;
- route and MAC table utilization;
- latency and packet loss between fabric nodes;
- overlay tunnel and EVPN route state where applicable.

Streaming telemetry is often more useful than slow polling for microbursts and rapid convergence events.

## Troubleshooting workflow

### 1. Confirm scope

Determine whether the issue affects one endpoint, one leaf, one rack, one spine path, one VRF, or the whole fabric.

### 2. Check physical state

```text
show interfaces status
show interfaces counters errors
show interfaces transceiver
```

### 3. Check underlay adjacency

```text
show bgp summary
show ospf neighbor
show bfd peers
```

### 4. Check routes and ECMP

```text
show ip route <destination>
show forwarding route <destination>
```

Confirm the expected number of next hops.

### 5. Test loopback and path reachability

Use sourced pings and traceroute from the relevant VRF or routing instance.

### 6. Check overlay state

Inspect EVPN routes, tunnel endpoints, VNIs, VRFs, MAC entries, and ARP or neighbor-discovery state.

### 7. Compare intended and actual configuration

Use the source of truth, generated configuration, and recent change history. Avoid making one-off CLI changes that create drift.

## Common problems

| Symptom | Likely checks |
|---|---|
| Only one rack affected | Leaf state, server links, leaf uplinks, rack power |
| Reduced throughput | Missing ECMP paths, optic errors, hashing, oversubscription |
| Intermittent loss | Bad optic, MTU mismatch, partial link failure, microbursts |
| Overlay unreachable | Underlay loopbacks, VTEP reachability, EVPN sessions, VNI mapping |
| One large flow is slow | Per-flow hashing and path utilization |
| Routes missing on one leaf | BGP policy, address family, max-prefix, session state |
| MLAG-host failure | Peer link, keepalive, split brain, LACP state |

## Design checklist

- Define rack, leaf, spine, and pod failure domains.
- Select underlay and overlay protocols deliberately.
- Calculate port count and oversubscription for current and future demand.
- Confirm ECMP width and forwarding-table capacity.
- Define loopback, point-to-point, ASN, VNI, VLAN, and VRF allocation.
- Standardize MTU.
- Decide how servers and appliances are multihomed.
- Build deterministic cabling and interface maps.
- Automate configuration from a source of truth.
- Test maintenance and failure scenarios before production.
- Monitor both control-plane convergence and data-plane forwarding.

## References

- [RFC 7938: Use of BGP for Routing in Large-Scale Data Centers](https://www.rfc-editor.org/rfc/rfc7938)
- [RFC 8365: A Network Virtualization Overlay Solution Using EVPN](https://www.rfc-editor.org/rfc/rfc8365)
- [RFC 7348: Virtual eXtensible Local Area Network](https://www.rfc-editor.org/rfc/rfc7348)
