# Leaf-Spine Network Architecture Cheat Sheet

## Introduction to Leaf-Spine Architecture

Leaf-spine is a two-layer network topology composed of leaf switches and spine switches. It's designed to minimize latency and manage increasing east-west network traffic in data centers efficiently.

- **Function**: Provides high-bandwidth, low-latency connectivity across network nodes.
- **Use Cases**: Data center networking, cloud services, large enterprise networks.

## Key Components

- **Leaf Switches**: Connect to servers, storage systems, or other end-point devices. Each leaf switch is connected to every spine switch.
- **Spine Switches**: Backbone of the network, providing interconnectivity between leaf switches. Spine switches only connect to leaf switches — never to each other.
- **East-West Traffic**: Traffic that flows within the data center, typically between servers or storage systems. Leaf-spine is optimized for this traffic pattern.
- **North-South Traffic**: Traffic flowing in and out of the data center (client to server). Handled at the leaf layer via border or edge leaf switches.

## Design Principles

- **Scalability**: Easily scalable by adding more leaf or spine switches without redesigning the network.
- **Reduced Latency**: Any two endpoints are always exactly two hops apart (leaf → spine → leaf), giving predictable, uniform latency.
- **Load Balancing**: Traffic is distributed evenly across all spine switches using ECMP (Equal-Cost Multi-Path) routing.
- **Non-blocking Architecture**: Designed to avoid network congestion by ensuring sufficient bandwidth at every stage.
- **No Spanning Tree**: Because leaf-spine uses IP routing (not bridging) between tiers, Spanning Tree Protocol (STP) is eliminated at the fabric level.

## Implementation Considerations

- **Sizing**: Proper sizing of leaf and spine switches based on port capacity and throughput requirements.
  - Number of spine switches = oversubscription ratio target
  - Number of leaf switches = number of servers ÷ ports per leaf
- **Redundancy**: Each leaf connects to every spine, providing inherent path redundancy. Loss of a spine switch only reduces bandwidth, not connectivity.
- **Cabling**: Efficient cabling strategies to manage the increased density of connections. Use structured cabling or pre-terminated fiber bundles.
- **Routing Protocols**: Use of routing protocols like BGP (preferred in modern designs) or OSPF for dynamic routing within the architecture.
- **Oversubscription**: Plan the ratio of downlink (server-facing) to uplink (spine-facing) bandwidth. Common targets are 3:1 or 4:1.

## BGP in Leaf-Spine (Modern Design)

BGP is increasingly the preferred routing protocol for leaf-spine fabrics, especially in large-scale environments:

- Each leaf and spine runs BGP.
- Leaf switches are eBGP peers with spine switches using unique ASNs per device.
- Benefits: simple, scalable, fine-grained policy control, and vendor-agnostic.
- Alternative: iBGP with route reflectors, or OSPF for smaller deployments.

## Benefits of Leaf-Spine Architecture

- **High Performance**: Optimized for high-bandwidth, low-latency networking.
- **Predictable Latency**: Uniform two-hop latency regardless of server location.
- **Flexibility**: Supports various types of traffic, including data, storage, and voice.
- **Simplified Management**: Easier to manage and troubleshoot compared to traditional three-tier (core/distribution/access) architectures.
- **Vendor Agnostic**: Works with any vendor's switches that support standard routing protocols.

## Comparison: Leaf-Spine vs Traditional Three-Tier

| Feature | Leaf-Spine | Three-Tier (Core/Dist/Access) |
|---|---|---|
| Latency | Uniform, 2 hops | Variable, 4–6 hops |
| East-West Traffic | Optimized | Not optimized |
| Scalability | High (add leaf or spine) | Limited by core capacity |
| Spanning Tree | Eliminated | Required |
| Complexity | Lower | Higher |
| Cost | Higher upfront | Lower upfront |

## Common Use Cases

- **Data Center Networks**: Ideal for modern data centers with high inter-server traffic.
- **Cloud Environments**: Supports the dynamic and scalable nature of cloud services.
- **High-Performance Computing**: Suitable for environments requiring fast computation and data retrieval.
- **Hyperconverged Infrastructure (HCI)**: Pairs well with HCI platforms like VMware vSAN or Nutanix.

## Deployment Tips

- **Network Virtualization**: Consider VXLAN overlays (with BGP EVPN) for multi-tenant environments and Layer 2 extension across leaf switches.
- **Capacity Planning**: Regularly review network utilization for capacity planning and upgrades. Monitor spine uplink utilization closely.
- **Monitoring and Analytics**: Implement network monitoring and analytics tools (e.g., streaming telemetry) for performance tracking and proactive issue detection.
- **Automation**: Leaf-spine fabrics are well suited to automation via tools like Ansible, Terraform, or vendor-specific APIs (e.g., Arista eAPI, Cisco NXAPI).
- **Test Before Production**: Validate ECMP behavior and failover scenarios in a lab or staging environment before production deployment.
