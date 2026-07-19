# OSI Model

> **Applies to:** General networking concepts
> **Last reviewed:** 2026-07-18

The Open Systems Interconnection (OSI) model is a seven-layer conceptual framework for discussing how applications communicate across networks. Real protocol implementations do not always map cleanly to one layer, but the model remains useful for design and troubleshooting.

## The seven layers

| Layer | Name | Primary concerns | Common examples |
|---:|---|---|---|
| 7 | Application | User-facing network services and application protocols | HTTP, DNS, SMTP, SSH, SNMP |
| 6 | Presentation | Encoding, serialization, compression, and encryption | TLS, JSON, ASN.1, character encodings |
| 5 | Session | Establishing and maintaining application conversations | RPC sessions, authentication sessions |
| 4 | Transport | End-to-end delivery, ports, reliability, and flow control | TCP, UDP, QUIC transport functions |
| 3 | Network | Logical addressing and routing between networks | IPv4, IPv6, ICMP, IPsec |
| 2 | Data link | Local delivery, framing, MAC addressing, and switching | Ethernet, VLANs, STP, LACP, ARP/ND adjacency functions |
| 1 | Physical | Signaling, media, connectors, optics, and bit transmission | Copper, fiber, radios, transceivers |

## Encapsulation

Application data is wrapped as it moves down the stack:

```text
application data
  -> transport segment or datagram
  -> network packet
  -> data-link frame
  -> physical signals
```

The receiving system removes the corresponding headers and trailers as data moves upward.

## Troubleshooting by layer

| Symptom | Start checking |
|---|---|
| No link or excessive physical errors | Layer 1: cable, optic, signal, speed, duplex |
| MAC flapping, VLAN mismatch, loop | Layer 2: switching, trunks, STP, LACP |
| Wrong route or unreachable subnet | Layer 3: address, mask, gateway, routing table, ACL |
| Connection reset, retransmission, port unreachable | Layer 4: TCP/UDP behavior, firewall, load balancer |
| TLS, authentication, or application error | Layers 5–7: session, encoding, certificates, application logs |

Do not stop at the layer where a symptom appears. For example, an application timeout may be caused by packet loss, MTU problems, DNS, TLS, or the application itself.

## Practical guidance

- Capture evidence at multiple layers before changing configuration.
- Separate control-plane state from data-plane forwarding.
- Use packet captures to confirm what is actually transmitted and received.
- Treat the OSI model as a communication tool rather than a rigid implementation specification.
