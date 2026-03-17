# QUIC, MASQUE & SRv6 — Datum Cloud Interview Cheat Sheet
### Network Engineering Reference — Focused on Datum's Open Network Cloud Architecture

---

## Table of Contents

1. [Datum Cloud — Architecture Overview](#datum-cloud--architecture-overview)
   - [What is Datum?](#what-is-datum)
   - [Founding Team & Lineage](#founding-team--lineage)
   - [The Three Pillars: Edge, Tunnels, Backbone](#the-three-pillars)
   - [Open Source Stack](#open-source-stack)
   - [Control Plane Architecture](#control-plane-architecture)
   - [Infrastructure Layout](#infrastructure-layout)
2. [QUIC](#quic)
   - [What is QUIC?](#what-is-quic)
   - [Why QUIC Exists — The Problems It Solves](#why-quic-exists)
   - [Protocol Stack Comparison](#protocol-stack-comparison)
   - [QUIC Internals](#quic-internals)
   - [Packet Structure](#quic-packet-structure)
   - [Connection Establishment](#quic-connection-establishment)
   - [Key Features In Depth](#quic-key-features-in-depth)
   - [QUIC & HTTP/3](#quic--http3)
   - [Iroh — How Datum Uses QUIC](#iroh--how-datum-uses-quic)
   - [Impact on Network Infrastructure](#impact-on-network-infrastructure)
3. [MASQUE](#masque)
   - [What is MASQUE?](#what-is-masque)
   - [Protocol Stack](#masque-protocol-stack)
   - [How MASQUE Works](#how-masque-works)
   - [MASQUE Mechanisms](#masque-mechanisms)
   - [Packet Structure & Encapsulation](#masque-packet-structure--encapsulation)
   - [Real-World Use Cases](#masque-real-world-use-cases)
   - [Relevance to Datum's Architecture](#masque-relevance-to-datum)
4. [SRv6](#srv6)
   - [What is SRv6?](#what-is-srv6)
   - [SRv6 vs MPLS — Why the Shift?](#srv6-vs-mpls)
   - [Core Concepts](#srv6-core-concepts)
   - [SRv6 Packet Structure](#srv6-packet-structure)
   - [SRH — Segment Routing Header](#srh--segment-routing-header)
   - [SRv6 Network Programming — Endpoint Behaviors](#srv6-network-programming)
   - [Micro-SID (uSID) — Compressed SRv6](#usid--compressed-srv6)
   - [SRv6 Encapsulation Modes](#srv6-encapsulation-modes)
   - [Control Plane](#srv6-control-plane)
   - [SRv6 TE — Traffic Engineering](#srv6-te--traffic-engineering)
   - [SRv6 for VPN Services](#srv6-for-vpn-services)
   - [How Datum Uses SRv6](#how-datum-uses-srv6)
   - [Real-World Deployment Use Cases](#srv6-real-world-deployment-use-cases)
5. [Datum's Full-Stack Architecture — How Everything Connects](#datums-full-stack-architecture)
   - [End-to-End Traffic Flow at Datum](#end-to-end-traffic-flow)
   - [The Operational Tension: SRv6 + QUIC Encryption](#operational-tension)
   - [Envoy — The Edge Proxy](#envoy--the-edge-proxy)
   - [HickoryDNS](#hickorydns)
   - [Crossplane — Cloud Provider Normalization](#crossplane)
   - [Galactic VPCs](#galactic-vpcs)
6. [Interview Talking Points — Key Concepts to Demonstrate](#interview-talking-points)
7. [Key RFCs & References](#key-rfcs--references)

---

# Datum Cloud — Architecture Overview

## What is Datum?

Datum is an **open-source network cloud** — a neutral platform where alt clouds, tech incumbents, and digital leaders can programmatically interact with their unique ecosystem. Datum provides the networking "superpowers" that large companies like hyperscalers and CDNs have (global edges, private networks, deterministic routing, direct interconnection) and makes them available to every developer, startup, and AI agent.

The core thesis: the internet is becoming fragmented (the "Splinternet") as regulations, sovereignty requirements, and the explosion of alt-cloud providers make the flat, open internet model obsolete. At the same time, AI agents and modern applications deploy everywhere — not just `us-east-1`. Datum provides the private networking layer that these applications need without requiring a dedicated network team.

> **One-liner**: Datum is what you'd build if you wanted to give every AI agent and alt-cloud provider the same networking capabilities that Cloudflare, AWS, and Equinix have — global edge, encrypted tunnels, deterministic backbone routing — as a self-service, open-source platform.

---

## Founding Team & Lineage

Datum was founded by industry veterans from **Voxel, Packet, Equinix, Fastly, and StackPath**. The CEO, Zac Smith, co-founded Packet (bare-metal cloud automation, acquired by Equinix for $335M in 2020) and previously ran Equinix Metal. The team has deep experience in bare-metal infrastructure, peering/interconnection, edge computing, and SP-grade networking.

This lineage matters for understanding Datum's architecture — these are people who deeply understand:
- Physical internet infrastructure (data centers, IXPs, cross-connects)
- Peering and transit economics
- SP-grade network programmability (which is why SRv6 is central to the backbone)
- Developer-first API design (the Packet philosophy)

The company is licensed under **AGPLv3** for core components.

---

## The Three Pillars

Datum's product architecture has three foundational networking layers:

```
┌─────────────────────────────────────────────────────────────────────────┐
│                                                                         │
│  ┌──────────────────────┐  ┌──────────────────────┐  ┌──────────────┐   │
│  │      1. EDGE         │  │     2. TUNNELS       │  │  3. BACKBONE │   │
│  │                      │  │                      │  │              │   │
│  │  Envoy-based proxy   │  │  QUIC P2P tunnels    │  │  SRv6 overlay│   │
│  │  + Coraza WAF        │  │  built with Iroh     │  │  "fast lanes"│   │
│  │  + AI Gateway        │  │  + NAT traversal     │  │  + VPC-style │   │
│  │                      │  │  + zero trust certs  │  │  control     │   │
│  │  17+ global PoPs     │  │  + hole punching     │  │  + telemetry │   │
│  └──────────────────────┘  └──────────────────────┘  └──────────────┘   │
│                                                                         │
│                    Kubernetes-native Control Plane                      │
│                    (CRDs + Crossplane + Controllers)                    │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Pillar 1: Edge
An intelligent **Envoy proxy** layer that protects and routes global internet traffic to backend services. Includes a WAF (Coraza, open-source), bot/DDoS protection, and the **Envoy AI Gateway** for routing traffic from application clients to Generative AI services. Deployed across 17+ global points of presence.

### Pillar 2: Tunnels (Connectors)
**Peer-to-peer QUIC tunnels** built on **Iroh** (a Rust library from n0.computer). Zero trust, certificate-based routing. The primary use case today is safely exposing localhost/internal services to the internet. Iroh handles:
- NAT traversal via UDP hole punching
- Relay server fallback (when direct P2P fails)
- End-to-end encryption (relay cannot inspect payload)
- Connection migration across network changes

### Pillar 3: Backbone
**SRv6 overlay** providing deterministic, auditable internet "fast lanes." This is the private backbone — controllable routing paths with VPC-style control and real-time observability (OpenTelemetry metrics export to Grafana Cloud). Unlike traditional private networks that take years to build, Datum aims to make virtual backbones available in minutes.

---

## Open Source Stack

Datum leans heavily into open-source projects. Key components:

| Component | Role in Datum | Technology |
|---|---|---|
| **Datum** (core) | System for managing/connecting network and data workloads | AGPLv3, Kubernetes-native |
| **Iroh** | P2P QUIC connectivity, NAT traversal, tunnels | Rust, fork of Quinn (QUIC impl) |
| **Envoy** | Edge proxy, service mesh, traffic routing | C++, CNCF graduated |
| **Envoy AI Gateway** | Routes traffic to GenAI services | Extension of Envoy Gateway |
| **Crossplane** | Control plane framework, normalizes cloud provider integrations | Kubernetes CRDs |
| **HickoryDNS** | Authoritative DNS service across 17 locations | Rust, safe/secure DNS |
| **SRv6** | Backbone deterministic routing via Segment Routing Header | IPv6 extension headers |
| **Milo** | "Business OS" for product-led B2B companies | Internal tooling |
| **Coraza WAF** | Web Application Firewall at the edge | Open source, OWASP compatible |

---

## Control Plane Architecture

Datum's control plane is built on **Kubernetes Custom Resource Definitions (CRDs)**. This is a deliberate architectural choice:

**Why Kubernetes as the control plane?**
- Extensibility: CRDs for simple schema-backed types, API Aggregation for custom server behavior
- Built-in authn/authz (OIDC, RBAC, Webhooks) with policy in the API
- Audit logs out of the box
- Reconciliation loops (controllers) — infrastructure self-heals to desired state
- Ecosystem familiarity: kubectl, watches, admission webhooks, OpenAPI discovery
- LLM compatibility: Claude, GPT, etc. have deep familiarity with the Kubernetes codebase

**How it works:**
```
User (datumctl / kubectl / Datum MCP for Cursor / API)
    │
    ▼
Kubernetes API Server (CRDs define Datum resources)
    │
    ├── Organizations → Projects (hierarchy with inherited policy)
    ├── Network resources (VPCs, tunnels, routes, DNS)
    ├── Edge resources (HTTPProxy, WAF rules)
    ├── Compute resources
    │
    ▼
Crossplane Controllers
    │
    ├── Reconcile desired state → actual state
    ├── Normalize across cloud providers (AWS, Azure, GCP, bare metal)
    └── Continuous reconciliation (drift detection, auto-repair)
```

This approach follows **KEP-4080** (Kubernetes Enhancement Proposal) — factoring out the generic parts of kube-apiserver and kube-controller-manager into reusable libraries for building control planes that don't need container orchestration semantics.

The tooling includes:
- **datumctl**: CLI with K8s-style syntax (get, apply, delete)
- **Datum MCP**: Official Model Context Protocol server for Cursor integration
- **Developer Tools**: macOS, Windows, Linux desktop apps

---

## Infrastructure Layout

Datum deploys infrastructure in **top internet aggregation points globally** using a region/AZ model:

| Field | Description | Example |
|---|---|---|
| Geography | ISO country code | US, DE, IN |
| Cardinal Direction | north, south, east, west, central | west |
| Number | Region index | 1 |
| Count | Availability Zone | a, b, c |

Example: `us-west-1a`

Each Region = geographic and network boundary. Each AZ = independent capacity within that Region. Currently 17+ global PoPs.

---

# QUIC

## What is QUIC?

QUIC is a general-purpose, encrypted transport protocol standardized by the IETF in **RFC 9000 (May 2021)**. It runs over **UDP** and is designed to replace TCP+TLS for latency-sensitive, high-performance applications.

Originally developed by Google (~2012) as an internal protocol for Chrome and Google services, it was adopted and standardized by the IETF with significant changes. The IETF version of QUIC is distinct from Google's original "gQUIC."

> **One-liner**: QUIC is what you get when you take TCP + TLS 1.3 + HTTP/2 multiplexing, throw out 40 years of TCP legacy, and rebuild it cleanly over UDP with encryption mandatory from the ground up.

> **Datum context**: QUIC is the transport foundation for Datum's tunnel/connector layer via Iroh — every P2P connection between Datum endpoints is a QUIC connection, not TCP.

---

## Why QUIC Exists

### The Problems QUIC Solves

**1. TCP Head-of-Line (HoL) Blocking**
- HTTP/2 multiplexes many streams over a single TCP connection
- If one TCP segment is lost, **all streams stall** waiting for retransmission — even streams with no missing data
- TCP sees one byte stream; it has no concept of independent application streams

```
TCP + HTTP/2 HoL Blocking:

Stream A: [chunk1] [chunk2] [  LOST  ] [chunk4]  ← all streams wait here
Stream B: [chunk1] [chunk2]   STALLED   STALLED
Stream C: [chunk1]            STALLED   STALLED
```

**2. TCP Handshake Latency**
- TCP 3-way handshake = 1 RTT before any data flows
- TLS 1.3 adds another 1 RTT on top (2 RTT total for first connection)
- TLS 1.3 resumed sessions: still 1 RTT (TCP) + 0 RTT (TLS) = 1 RTT

**3. Ossification**
- Middleboxes (firewalls, NATs, LBs, DPI) have learned to read and manipulate TCP headers
- TCP protocol cannot evolve because middleboxes break on unknown options
- QUIC solves this by **encrypting nearly everything** — middleboxes see only UDP + QUIC Connection ID

**4. Connection Migration**
- TCP connections = {src IP, src port, dst IP, dst port} 4-tuple
- Network change (Wi-Fi → LTE) = source IP changes → TCP breaks → reconnect needed
- QUIC uses **Connection IDs** — seamless migration across network changes
- **Datum relevance**: This is critical for Iroh — P2P tunnels survive network transitions without reconnecting

**5. No Kernel Involvement for Protocol Evolution**
- TCP lives in the OS kernel; changes require OS upgrades
- QUIC runs in userspace — updates ship as fast as application updates

---

## Protocol Stack Comparison

```
Traditional HTTPS          HTTP/2              HTTP/3 (QUIC)
─────────────────          ──────────────      ─────────────────────────

┌─────────────┐           ┌───────────┐       ┌─────────────────────────┐
│   HTTP/1.1  │           │  HTTP/2   │       │        HTTP/3           │
├─────────────┤           ├───────────┤       ├─────────────────────────┤
│    TLS 1.2  │           │  TLS 1.3  │       │         QUIC            │
├─────────────┤           ├───────────┤       │  (streams + crypto +    │
│     TCP     │           │    TCP    │       │   flow control + CC)    │
├─────────────┤           ├───────────┤       ├─────────────────────────┤
│     IP      │           │    IP     │       │           UDP           │
└─────────────┘           └───────────┘       ├─────────────────────────┤
                                              │           IP            │
                                              └─────────────────────────┘

RTT to first byte:          RTT to first byte:  RTT to first byte:
New:    2 RTT               New:    2 RTT        New:    1 RTT
Resume: 1 RTT               Resume: 1 RTT        Resume: 0 RTT (0-RTT)
```

---

## QUIC Internals

### Connection Lifecycle

```
Client                              Server
  │                                    │
  │──── Initial (ClientHello) ────────>│  ← Crypto handshake starts immediately
  │                                    │    in the first packet (no separate TCP SYN)
  │<─── Initial (ServerHello) ─────────│
  │<─── Handshake (cert, finish) ──────│
  │                                    │
  │──── Handshake (finish) ───────────>│  ← 1 RTT: connection established + encrypted
  │                                    │
  │<═══ Application Data ══════════════│  ← Data flows immediately after
  │═══> Application Data ══════════════│
  │                                    │
  │  (0-RTT resumption on next conn)   │
  │──── 0-RTT Data + ClientHello ─────>│  ← 0 RTT: data sent before server responds
```

### QUIC Packet Number Spaces

QUIC uses three separate packet number spaces, each with its own encryption level:

| Space | Encryption | Purpose |
|---|---|---|
| Initial | AEAD with fixed key (from spec) | Bootstrap crypto — Client/Server Hello |
| Handshake | AEAD with handshake keys | Certificate, Finished messages |
| Application (1-RTT) | AEAD with session keys | All application data + most QUIC frames |

Packet loss in one space doesn't affect others — each space has independent ACK and retransmission.

### Streams

QUIC multiplexes independent, ordered byte streams over a single connection:

- Each stream is independently flow-controlled
- Loss in Stream A does **not** block Stream B (no HoL blocking)
- Stream types:
  - **Bidirectional** (client-initiated: 0, 4, 8... / server-initiated: 1, 5, 9...)
  - **Unidirectional** (client-initiated: 2, 6, 10... / server-initiated: 3, 7, 11...)
- Stream IDs are 62-bit variable-length integers

```
Single QUIC Connection — Multiple Independent Streams:

          ┌──────────────────────────────────┐
          │         QUIC Connection          │
          │  Connection ID: 0xA3F2...        │
          │                                  │
          │  Stream 0 ────────────────────>  │  (request 1)
          │  Stream 4 ──────────>            │  (request 2)
          │  Stream 8 ──────────────────>    │  (request 3)
          │  Stream 1 <────────────────────  │  (server push)
          │                                  │
          │  Each stream: independent ACK,   │
          │  flow control, retransmission    │
          └──────────────────────────────────┘
```

### Congestion Control

- QUIC does not mandate a specific CC algorithm
- Default: **NewReno** or **CUBIC** (same as TCP)
- Userspace implementation means **BBR** can be deployed without OS changes — important for high-BDP paths
- QUIC packet numbering eliminates retransmission ambiguity (RFC 9002)

### Connection Migration

```
Mobile Client                          Server
   │                                      │
   │  [on Wi-Fi, src: 192.168.1.5:4321]  │
   │═══════════════════════════════════>  │  CID: 0xDEADBEEF
   │                                      │
   │  [switches to LTE, src: 10.0.0.1:9876]
   │                                      │
   │══ PATH_CHALLENGE ════════════════>   │  ← Client validates new path
   │<═ PATH_RESPONSE ══════════════════   │
   │                                      │
   │═══════════════════════════════════>  │  CID: 0xDEADBEEF (unchanged)
   │  [LTE, same Connection ID]           │  ← No reconnect. No re-handshake.
```

---

## QUIC Packet Structure

### Long Header Packet (handshake)

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
├─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┤
│1│ Type  │ Reserved  │ PKT# Len  │   Version (32 bits)           │
├─┴───────┴───────────┴───────────┼───────────────────────────────┤
│  DCID Len (8)  │  Dest Conn ID (0-20 bytes)                    │
├────────────────┼───────────────────────────────────────────────┤
│  SCID Len (8)  │  Source Conn ID (0-20 bytes)                  │
├────────────────┴───────────────────────────────────────────────┤
│  Type-specific fields (Token, Length, Packet Number)           │
├────────────────────────────────────────────────────────────────┤
│  Payload (AEAD-encrypted frames)                               │
└────────────────────────────────────────────────────────────────┘

Header bit 1 = 1 → Long Header (handshake/version negotiation)
Types: Initial (0x00), 0-RTT (0x01), Handshake (0x02), Retry (0x03)
```

### Short Header Packet (application data — 1-RTT)

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
├─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┼─┤
│0│1│S│R│R│K│ PKT# │  Dest Connection ID (variable, 0-20 bytes)  │
├─┴─┴─┴─┴─┴─┴──────┴────────────────────────────────────────────┤
│  Packet Number (1-4 bytes)                                     │
├────────────────────────────────────────────────────────────────┤
│  Payload (AEAD-encrypted QUIC frames)                          │
└────────────────────────────────────────────────────────────────┘

S = Spin bit (for passive RTT measurement by network devices)
K = Key phase bit (tracks key rotation)
Header bit 0 = 0 → Short Header (1-RTT application data)

What a middlebox can see: UDP src/dst port, Connection ID, Spin bit
Everything else is encrypted — including stream IDs, frame types, payload
```

### Key QUIC Frame Types (inside encrypted payload)

| Frame Type | Purpose |
|---|---|
| STREAM | Carries application stream data |
| ACK | Acknowledges received packets |
| CRYPTO | Carries TLS handshake data |
| CONNECTION_CLOSE | Terminates connection |
| MAX_DATA | Flow control — connection level |
| MAX_STREAM_DATA | Flow control — stream level |
| NEW_CONNECTION_ID | Provides new CIDs (for migration) |
| PATH_CHALLENGE / PATH_RESPONSE | Validates new network path |
| PING | Keeps connection alive |
| HANDSHAKE_DONE | Signals handshake completion (server→client) |
| DATAGRAM (RFC 9221) | Unreliable datagrams within a QUIC connection |

---

## QUIC Connection Establishment

### First Connection (1 RTT)

```
t=0  Client sends Initial packet:
       - CRYPTO frame carrying TLS ClientHello
       - src/dst CIDs chosen by client
       - Token (empty on first connection)

t=0.5 RTT  Server responds:
       - Initial: CRYPTO (ServerHello)
       - Handshake: CRYPTO (EncryptedExtensions, Certificate, CertVerify, Finished)
       - Can also send 1-RTT data early

t=1 RTT  Client sends:
       - Handshake: CRYPTO (Finished) — handshake complete
       - 1-RTT: Application data begins flowing

Total: 1 RTT to first application byte
```

### Resumed Connection (0-RTT)

```
t=0  Client sends:
       - Initial: CRYPTO (ClientHello with session ticket)
       - 0-RTT: Application data (sent immediately, before server responds)
       ← Encrypted with keys from previous session

t=0.5 RTT  Server responds and can immediately handle 0-RTT data

Total: 0 RTT — application data sent before server's first response

Caveat: 0-RTT data is vulnerable to replay attacks.
        Servers must handle 0-RTT data idempotently.
        Non-idempotent requests (e.g., payments) should not use 0-RTT.
```

---

## QUIC Key Features In Depth

### The Spin Bit — Passive RTT Measurement

The spin bit (bit 2 of short header) toggles once per RTT, allowing passive RTT measurement without decryption:

```
Client →→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→ Server
spin=0 →→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→
                                             spin=1 ←←←←←←←←←←←
spin=1 →→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→→
                                             spin=0 ←←←←←←←←←←←

Network tap measures time between transitions = 1 RTT
This is the ONLY latency signal available to middleboxes in QUIC.
```

**Datum relevance**: On the SRv6 backbone, the spin bit is one of the few observability signals available for QUIC flows — critical for the backbone's telemetry and performance monitoring.

### Loss Recovery

- TCP reuses sequence numbers for retransmissions → ambiguity about which transmission was ACKed
- QUIC uses **monotonically increasing packet numbers** — retransmitted data gets a new number
- Eliminates retransmission ambiguity, enables accurate RTT measurement during loss (RFC 9002)

### QUIC Versions

| Version | Status | Notes |
|---|---|---|
| Draft versions (< 29) | Obsolete | Google gQUIC, pre-standardization |
| QUIC v1 (0x00000001) | RFC 9000 — Current standard | What HTTP/3 and Iroh use |
| QUIC v2 (0x6b3343cf) | RFC 9369 | Minor improvements, greasing version ossification |

---

## QUIC & HTTP/3

HTTP/3 is HTTP semantics (methods, headers, status codes) carried over QUIC instead of TCP.

```
HTTP/3 internal structure over QUIC:

QUIC Stream 0 (bidi, client-init): HTTP/3 request/response
QUIC Stream 4 (bidi, client-init): Another HTTP/3 request/response
QUIC Stream 2 (uni, client-init): QPACK encoder stream
QUIC Stream 6 (uni, client-init): HTTP/3 control stream
QUIC Stream 3 (uni, server-init): QPACK decoder stream
QUIC Stream 7 (uni, server-init): HTTP/3 control stream (server)
```

**QPACK** replaces HTTP/2's HPACK for header compression. Designed for out-of-order delivery on QUIC streams (HPACK required ordered processing, which was itself a HoL blocking risk).

---

## Iroh — How Datum Uses QUIC

This is the section most directly relevant to Datum's architecture. Iroh is not HTTP/3 — it's a **connectivity library** that uses QUIC as a raw transport for building P2P protocols.

### What is Iroh?

Iroh is a Rust library built by **n0.computer** that establishes direct peer-to-peer QUIC connections. It uses a fork of **Quinn**, a pure-Rust QUIC implementation. Datum uses Iroh for its "Connectors" / tunnel layer.

### How Iroh P2P Connectivity Works

```
Endpoint A                    Relay Server                    Endpoint B
(NodeId: Ed25519 pubkey)      (HTTP/1.1 over TLS)            (NodeId: Ed25519 pubkey)
    │                              │                              │
    │── Register (HTTP) ──────────>│<────── Register (HTTP) ──────│
    │                              │                              │
    │   [Phase 1: Relay fallback — works immediately]             │
    │══ UDP datagram ══════════════│══ forward to B ══════════════>│
    │   (encrypted to B's NodeId)  │  (relay cannot read payload) │
    │                              │                              │
    │   [Phase 2: Hole punching attempt — runs in background]     │
    │                              │                              │
    │══ simultaneous UDP ══════════════════════════════════════>   │
    │<════════════════════════════════════════ simultaneous UDP ═══│
    │                              │                              │
    │   [If hole punch succeeds: direct P2P QUIC connection]      │
    │<════════════════════════════════════════════════════════════>│
    │   [Relay drops out, connection is now direct]                │
```

### Key Iroh/QUIC Design Decisions

| Property | Detail |
|---|---|
| **Identity** | Ed25519 NodeId (cryptographic, not IP-based) |
| **NAT traversal** | UDP hole punching via relay coordination |
| **Fallback** | Relay server forwards encrypted UDP datagrams |
| **Relay security** | Payload always encrypted to destination — relay is blind |
| **Relay protocol** | HTTP/1.1 over TLS (most universally supported protocol) |
| **QUIC implementation** | Quinn (pure Rust), forked and maintained by n0.computer |
| **Multipath** | Active work on QUIC-MULTIPATH IETF draft in Quinn fork |
| **Stream API** | Exposes full QUIC stream semantics (bidi, uni, finish, reset, stop) |

### Why QUIC for P2P (Not Just HTTP/3)

Iroh deliberately exposes QUIC's stream API rather than abstracting it away because:
- QUIC is more expressive than TCP — multiple concurrent streams, fine-grained flow control, cancellation
- Protocol design matters — file transfer needs different patterns than chat or real-time collaboration
- Bidirectional streams can be stopped/reset as a unit
- Stream IDs are monotonically increasing — can determine ordering without application-level sequencing

### Iroh vs Traditional VPN / Tunnel Approaches

| | Traditional VPN | Iroh / Datum Tunnels |
|---|---|---|
| Architecture | Client-server, centralized gateway | P2P, decentralized |
| Identity | IP-based or certificate to gateway | Ed25519 cryptographic identity |
| NAT traversal | Requires gateway with public IP | Hole punching + relay fallback |
| Encryption overhead | IPsec/WireGuard encapsulation | QUIC TLS 1.3, native |
| Connection migration | Reconnect on network change | QUIC CID — seamless |
| Multiplexing | Single tunnel for all traffic | Multiple independent QUIC streams |
| Transport | TCP/UDP encapsulation | Native QUIC over UDP |
| Trust model | Trust the VPN server | Zero trust — relay cannot inspect |

---

## Impact on Network Infrastructure

### What Changes for Network Engineers

**Firewall / ACL considerations:**
- QUIC runs on **UDP 443** by default. Firewalls that rate-limit or block UDP 443 will break QUIC
- QUIC uses ephemeral UDP ports — existing UDP tracking rules may need tuning

**DPI / Traffic classification:**
- QUIC encrypts almost everything. Connection IDs are the only stable identifier
- SNI in Initial packet's CRYPTO frame is **not** encrypted in QUIC v1 (same as TLS 1.3). ECH will change this
- Flow classification tools need QUIC-specific parsers

**Load balancer design:**
- 4-tuple routing breaks with connection migration
- QUIC-aware LBs use **Server Connection IDs** encoding routing info (RFC 9484 — QUIC-LB)
- Consistent hashing on Connection ID required

**NAT behavior:**
- UDP NAT bindings time out faster than TCP (typically 30s vs 300s)
- QUIC uses **PING frames** to keep NAT bindings alive
- NAT timeout mismatches are a common operational issue

**Monitoring:**
- Spin bit = your RTT signal
- Loss rate inferred from packet number gaps
- Flow-level telemetry (IPFIX/NetFlow) rather than payload inspection

---

# MASQUE

## What is MASQUE?

**MASQUE** (Multiplexed Application Substrate over QUIC Encryption) is an IETF framework (RFC 9297, RFC 9298, RFC 9484) for building proxying and tunneling services on top of **HTTP/3 and QUIC**.

It provides a standardized, encrypted mechanism for:
- **IP tunneling** (full Layer 3 proxy — like a VPN)
- **UDP proxying** (relay UDP datagrams — like TURN, but standardized)
- **TCP proxying** (CONNECT method, extended to QUIC)

> **One-liner**: MASQUE is what you'd build if you needed to replace an MPLS VPN, GRE tunnel, or IPsec VPN with something that runs natively over HTTP/3, looks like regular HTTPS traffic, and supports multiplexing many tunnels over one QUIC connection.

> **Datum context**: While Datum's current tunnel implementation uses Iroh (raw QUIC P2P), MASQUE represents the IETF-standardized approach to the same problem space. Understanding MASQUE provides context for the broader industry direction and potential future integration patterns.

---

## MASQUE Protocol Stack

```
┌─────────────────────────────────────────────────────────────┐
│                    Proxied Traffic                          │
│          (IP packets, UDP datagrams, TCP streams)           │
├─────────────────────────────────────────────────────────────┤
│                   MASQUE Protocol                           │
│   (Connect-IP / Connect-UDP / CONNECT / HTTP Datagrams)     │
├─────────────────────────────────────────────────────────────┤
│                       HTTP/3                                │
├─────────────────────────────────────────────────────────────┤
│                        QUIC                                 │
├─────────────────────────────────────────────────────────────┤
│                        UDP                                  │
├─────────────────────────────────────────────────────────────┤
│                        IP                                   │
└─────────────────────────────────────────────────────────────┘
```

---

## How MASQUE Works

MASQUE extends the HTTP **CONNECT** method to support UDP and IP:

| Method | RFC | Tunnels |
|---|---|---|
| `CONNECT` (classic) | RFC 9110 | TCP streams |
| `CONNECT-UDP` | RFC 9298 | UDP datagrams |
| `CONNECT-IP` | RFC 9484 | IP packets (full L3 tunnel) |

### Basic Flow

```
Client                    MASQUE Proxy                   Target
  │                            │                            │
  │─── HTTP/3 CONNECT-UDP ────>│                            │
  │    Target: 10.0.0.1:5004  │                            │
  │                            │                            │
  │<─── 200 OK ────────────────│  (tunnel established)      │
  │                            │                            │
  │═══ HTTP Datagram ══════════│══> UDP packet ════════════>│
  │    (UDP payload            │    to 10.0.0.1:5004        │
  │     inside QUIC DATAGRAM)  │                            │
  │                            │<══ UDP response ═══════════│
  │<══ HTTP Datagram ══════════│                            │

All Client↔Proxy traffic: encrypted QUIC, looks like HTTPS (UDP 443)
```

---

## MASQUE Mechanisms

### HTTP Datagrams (RFC 9297)

Core data transport for MASQUE using QUIC **DATAGRAM** frames (RFC 9221):

```
QUIC DATAGRAM frame:
┌──────────────────────────────────────────────────────┐
│  Quarter Stream ID (variable-length int)             │
├──────────────────────────────────────────────────────┤
│  HTTP Datagram Payload                               │
│  (e.g., UDP payload, IP packet)                      │
└──────────────────────────────────────────────────────┘

Key: UNRELIABLE delivery (no retransmission)
→ Avoids double-retransmission for already-reliable traffic
```

### Capsule Protocol (Fallback)

When QUIC DATAGRAM frames are unavailable (restrictive middleboxes), MASQUE falls back to the **Capsule Protocol** — datagrams inside HTTP/3 DATA frames on a reliable stream:

```
Trade-off: works through restrictive firewalls but adds HoL blocking
```

### CONNECT-IP — Full IP Tunneling (RFC 9484)

```
Client                      MASQUE Proxy                  Internet
  │                              │                            │
  │  CONNECT-IP: "::/0"         │                            │
  │─────────────────────────────>│                            │
  │<── 200 OK (assigns IP) ─────│                            │
  │                              │                            │
  │══ IP packet (dst 8.8.8.8) ══>│═══ IP packet ════════════>│
  │   (inside HTTP Datagram)     │   (NAT or route)           │
  │                              │<══ IP response ════════════│
  │<═ IP packet (src 8.8.8.8) ══│                            │

Result: Client has a virtual IP, full internet access via proxy.
        To observers: looks like HTTPS traffic to proxy IP.
```

---

## MASQUE Packet Structure & Encapsulation

### Overhead Analysis

| Component | Bytes |
|---|---|
| IPv6 outer header | 40 |
| UDP outer header | 8 |
| QUIC short header (min) | ~20 |
| QUIC DATAGRAM frame overhead | ~4 |
| Quarter Stream ID | 1–4 |
| **Total overhead (approx)** | **~75–80 bytes** |

Comparison: IPsec ESP tunnel mode ≈ 50–70 bytes. MASQUE is slightly heavier but adds TLS 1.3 encryption + HTTP/3 multiplexing.

---

## MASQUE Real-World Use Cases

### iCloud Private Relay
Apple's two-hop MASQUE architecture:
- Hop 1: Apple ingress proxy (knows user identity, not destination)
- Hop 2: Third-party egress proxy (knows destination, not user identity)
- No single party knows both who the user is and where they're going

### Zero Trust Network Access (ZTNA)
- Looks like normal HTTPS — passes through restrictive firewalls
- Per-application tunnels (not full VPN) — CONNECT-UDP per app
- QUIC multiplexing — many application tunnels over one connection

### 5G ATSSS (Access Traffic Steering, Switching, Splitting)
- ATSSS proxy uses MASQUE CONNECT-UDP for multi-path traffic steering between Wi-Fi and cellular

---

## MASQUE Relevance to Datum

| Aspect | Datum's Approach | MASQUE Standard |
|---|---|---|
| Tunneling | Iroh (raw QUIC, P2P) | HTTP/3 extended CONNECT |
| Proxy model | Relay server (blind forward) | MASQUE proxy (protocol-aware) |
| NAT traversal | Hole punching + relay | CONNECT through HTTP proxy |
| Encryption | QUIC TLS 1.3 (Iroh) | QUIC TLS 1.3 (HTTP/3) |
| Multiplexing | QUIC streams | HTTP/3 + QUIC streams |

While Datum currently uses Iroh rather than MASQUE, the concepts overlap significantly. MASQUE represents the IETF-standardized path for encrypted tunneling over HTTP/3, and future Datum features (like the "Galactic VPC" L3 connectivity) could draw on CONNECT-IP patterns.

---

# SRv6

## What is SRv6?

**SRv6** (Segment Routing over IPv6) is a source-based routing and network programming architecture where:
1. The **source node** encodes routing instructions (**segments**) directly into the IPv6 extension header
2. Intermediate nodes execute the instructions — **no per-flow state** in the network
3. Each segment is a **128-bit IPv6 address** encoding both a locator (where to route) and a function (what to do)

Standardized by IETF Spring WG: RFC 8754 (SRH), RFC 8986 (SRv6 Network Programming).

> **One-liner**: SRv6 is MPLS done with native IPv6 — no labels, no LDP, no RSVP-TE. The "label stack" is a list of IPv6 addresses in the packet header, and each hop executes a function based on its local SID definition.

> **Datum context**: SRv6 is the underlay for Datum's backbone — the "fast lanes" that provide deterministic, auditable routing across Datum's global PoP infrastructure. This is not a 5G transport network; it's an open-source network cloud using SRv6 for **internet backbone programmability**.

---

## SRv6 vs MPLS

| Feature | MPLS | SRv6 |
|---|---|---|
| Data plane | 32-bit label stack | IPv6 extension header (SRH) |
| Control plane | LDP, RSVP-TE, BGP-LU | IS-IS, OSPF, BGP (with SID advertisements) |
| Per-flow state | Required for RSVP-TE tunnels | None (stateless forwarding) |
| Interoperability | Requires MPLS support end-to-end | Native IPv6 — any IPv6-capable device can transit |
| TE granularity | Tunnel-based | Per-packet source routing |
| VPN support | L3VPN (BGP VPNv4/v6) | BGP SRv6 L3VPN / EVPN |
| Overhead | 4 bytes per label | 16+ bytes per SID (larger, but no separate label plane) |
| Programmability | Limited | High — per-node function definition |
| OAM | MPLS OAM (RFC 8029) | Native IPv6 ping/traceroute works on SRv6 paths |

---

## SRv6 Core Concepts

### Segment Identifier (SID)

An SRv6 SID is a **128-bit IPv6 address** structured as:

```
┌────────────────────────────┬───────────────┬─────────────┐
│         Locator            │   Function    │    Args     │
│    (routable prefix)       │  (what to do) │  (optional) │
│      B bits                │    F bits     │   A bits    │
└────────────────────────────┴───────────────┴─────────────┘

Example: 2001:db8:1::/48 = Locator (node address prefix)
         0x0001          = Function (e.g., End.DT4 for IPv4 L3VPN)
         ::              = Args (empty)

Full SID: 2001:db8:1::1  →  End.DT4 function on node with locator 2001:db8:1::/48
```

The locator is advertised into the IGP (IS-IS/OSPF) so other nodes know how to reach it. The function is locally significant.

### Active Segment

The **active segment** = current destination of the packet. The SRH contains a **Segments Left** counter indicating which SID to process next.

### SRv6 Node Roles

| Role | Description |
|---|---|
| **Ingress Node (SR Source)** | Adds the SRH with the segment list |
| **Transit Node** | Forwards based on outer IPv6 DA (no SRH processing if not a SID) |
| **Endpoint Node** | Processes the active SID — executes the defined function |
| **Egress Node** | Last segment endpoint — delivers to final destination |

---

## SRv6 Packet Structure

```
 Standard IPv6 Packet with SRH:

┌─────────────────────────────────────────────────────────────┐
│                    IPv6 Base Header                         │
│  Version=6, Traffic Class, Flow Label                       │
│  Payload Length, Next Header=43 (Routing Header), Hop Limit │
│  Source Address: Ingress Node                               │
│  Destination Address: Current Active Segment (SID[n])       │
├─────────────────────────────────────────────────────────────┤
│              Segment Routing Header (SRH)                   │
│  Next Header, Hdr Ext Len, Routing Type = 4 (SRv6)         │
│  Segments Left: n (countdown to 0)                          │
│  Last Entry: index of last segment in list                  │
│  Flags (8 bits), Tag (16 bits)                              │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  Segment List[0]:  SID of last segment (final dest)   │  │
│  │  Segment List[1]:  SID of second-to-last segment      │  │
│  │  ...                                                  │  │
│  │  Segment List[n]:  SID of first segment (active now)  │  │
│  └───────────────────────────────────────────────────────┘  │
│  Optional TLVs (Padding, HMAC, etc.)                        │
├─────────────────────────────────────────────────────────────┤
│              Inner Payload                                  │
│  (IPv4/IPv6 packet, Ethernet frame, etc.)                   │
└─────────────────────────────────────────────────────────────┘

IMPORTANT: Segment list is ordered LAST to FIRST.
           Segment List[n] = currently active SID (in IPv6 DA)
           Segment List[0] = final destination SID
           Segments Left decrements as each node processes its SID.
```

### SRH Processing at Each Hop

```
Ingress Node:
  1. Build SRH with segment list [SID_C, SID_B, SID_A] (last→first order)
  2. Set Segments Left = 2
  3. Set IPv6 DA = SID_A (first segment = last in list)
  4. Forward packet

At SID_A (Segments Left = 2):
  1. Execute function for SID_A
  2. Segments Left-- → 1
  3. IPv6 DA = Segment List[1] = SID_B
  4. Forward packet

At SID_B (Segments Left = 1):
  1. Execute function for SID_B
  2. Segments Left-- → 0
  3. IPv6 DA = Segment List[0] = SID_C
  4. Forward packet

At SID_C (Segments Left = 0):
  1. Execute function for SID_C (typically deliver payload)
  2. Remove SRH, deliver inner packet to destination
```

---

## SRH — Segment Routing Header

```
 0                   1                   2                   3
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
├─────────────────┬───────────────┬───────────────┬───────────────┤
│   Next Header   │  Hdr Ext Len  │ Routing Type=4│ Segments Left │
├─────────────────┴───────────────┴───────────────┴───────────────┤
│   Last Entry    │    Flags      │            Tag                │
├─────────────────────────────────────────────────────────────────┤
│                   Segment List[0] (128 bits / 16 bytes)         │
│                   (Final Destination SID)                       │
├─────────────────────────────────────────────────────────────────┤
│                   ...                                           │
├─────────────────────────────────────────────────────────────────┤
│                   Segment List[n] (128 bits / 16 bytes)         │
│                   (First/Active SID — also in IPv6 DA)          │
├─────────────────────────────────────────────────────────────────┤
│                   Optional TLVs                                 │
└─────────────────────────────────────────────────────────────────┘

Overhead per SID = 16 bytes
SRH fixed overhead = 8 bytes
Total overhead for n segments = 8 + (n × 16) bytes

Example: 4-segment path = 8 + 64 = 72 bytes of SRH overhead
```

---

## SRv6 Network Programming

SRv6 defines **endpoint behaviors** — standardized functions executed by nodes at the active segment (RFC 8986):

| Behavior | Name | Description |
|---|---|---|
| `End` | Endpoint | Basic SID — update DA to next segment, forward |
| `End.X` | Endpoint with L3 cross-connect | Forward to specific L3 adjacency (TE use case) |
| `End.T` | Endpoint with Table lookup | Forward in specific routing table |
| `End.DX4` | Endpoint with decap + IPv4 cross-connect | Decap SRv6, forward IPv4 to specific NH (L3VPN egress) |
| `End.DX6` | Endpoint with decap + IPv6 cross-connect | Decap SRv6, forward IPv6 to specific NH |
| `End.DT4` | Endpoint with decap + IPv4 table lookup | Decap SRv6, lookup in VRF (BGP L3VPN) |
| `End.DT6` | Endpoint with decap + IPv6 table lookup | Decap SRv6, lookup IPv6 VRF |
| `End.DT46` | Endpoint with decap + IP table lookup | Decap SRv6, lookup IPv4 or IPv6 VRF |
| `End.DX2` | Endpoint with decap + L2 cross-connect | EVPN VPWS |
| `End.B6.Encaps` | Endpoint with SR policy encap | Re-encapsulate with a new SR policy |
| `End.BM` | Endpoint with SR in MPLS | SRv6 ↔ SR-MPLS interworking |

---

## uSID — Compressed SRv6

Standard SRv6 SIDs = 128 bits = 16 bytes per segment. For many-segment paths, overhead is significant. **uSID (micro-SID)** compresses multiple SIDs into a single 128-bit address:

```
Standard SID: 2001:db8:0001:0000:0000:0000:0000:0000 (one node, one action)

uSID:         2001:db8:0001:0002:0003:0000:0000:0000
              └─ locator ─┘└─u1─┘└─u2─┘└─u3─┘

Three micro-SIDs packed into one 128-bit address.
Each uSID = 16 bits (or 32 bits in some deployments).
Reduces overhead from 48 bytes (3 SIDs) to 16 bytes (1 SID with 3 uSIDs).
```

uSID is gaining rapid adoption (Cisco, Nokia, Juniper) — makes SRv6 overhead competitive with MPLS.

**Datum relevance**: For a global backbone with multiple PoP hops, uSID compression is essential to keep packet overhead reasonable and competitive with traditional MPLS-based approaches.

---

## SRv6 Encapsulation Modes

### H.Encaps (Encapsulation) — Most Common

```
┌─────────────────────────────┐
│ Outer IPv6 Header           │  DA = first active SID
├─────────────────────────────┤
│ SRH                         │  Segment list
├─────────────────────────────┤
│ Original IPv4/IPv6 Packet   │  Unchanged inner packet
└─────────────────────────────┘

Use case: PE routers in L3VPN, traffic engineering ingress
```

### H.Insert — SRH Insertion (no outer IPv6)

```
┌─────────────────────────────┐
│ Original IPv6 Header        │  DA updated to first SID
├─────────────────────────────┤
│ SRH (inserted)              │  Segment list
├─────────────────────────────┤
│ Original IPv6 Payload       │
└─────────────────────────────┘

Use case: Transit IPv6 traffic, native SRv6 endpoints
Note: Cannot be used for IPv4 traffic
```

---

## SRv6 Control Plane

### IGP SID Advertisement

SRv6 SIDs are advertised via IS-IS or OSPF extensions:

- **IS-IS**: SRv6 Locator TLV (TLV 27), SRv6 End SID Sub-TLV
- **OSPF**: SRv6 Locator LSA (Type 10), SRv6 SID Sub-TLV in Router-Info LSA

### BGP for SRv6 Services

BGP distributes service-layer SIDs (for L3VPN, EVPN):

```
BGP Update for SRv6 L3VPN:
  NLRI: VPNv4 prefix (e.g., 10.0.0.0/24, RD 100:1)
  Next-Hop: PE loopback (IPv6)
  Attribute: Prefix-SID → SRv6 SID (e.g., End.DT4 SID)

PE-2 receives this and knows:
  "To reach 10.0.0.0/24 in VRF 100:1,
   encapsulate with SRv6, set active SID to End.DT4 on PE-1"
```

### SR Policy

An SR Policy is an explicitly defined segment list (path) installed by:
- **PCE** (Path Computation Element) — centralized TE via PCEP
- **BGP SR Policy** — distributed via BGP UPDATE
- **Headend static config** — manually defined
- **Controller** — SDN controller (relevant for Datum's programmatic approach)

```
SR Policy:
  Endpoint: PE-2 (2001:db8:2::1)
  Color:    100 (TE constraint identifier)
  Candidate Paths:
    Priority 10: [End.X@Node-A via link1, End.X@Node-C, End.DT4@PE-2]
    Priority 20: [End.X@Node-B, End.DT4@PE-2]  (backup)
```

---

## SRv6 TE — Traffic Engineering

SRv6 TE uses **End.X SIDs** to steer traffic through specific links/nodes without RSVP-TE state:

```
Physical Topology:          SR Policy (low-latency path):

  A ─── B ─── D            Segment List: [End.X@A→C, End.X@C→D, End.DT4@D]
  │     │     │
  └── C ─────┘            Packet takes A→C→D (low latency path)
                           even though shortest IGP path might be A→B→D
```

### SRv6 TE vs RSVP-TE

| | RSVP-TE | SRv6 TE |
|---|---|---|
| State in network | Per-tunnel RSVP state on every node | No per-flow state anywhere |
| Signaling | RSVP PATH/RESV messages | IGP SID advertisement only |
| Scale | Thousands of tunnels = scalability problem | Millions of flows, no state |
| Failure recovery | RSVP FRR (fast reroute) | TI-LFA (Topology Independent LFA) |
| Flexibility | Tunnel-level TE | Per-packet source routing |

---

## SRv6 for VPN Services

### BGP SRv6 L3VPN

```
CE-1 → PE-1 (ingress):
  Lookup CE-1's route in VRF → matches BGP route with SRv6 SID from PE-2
  Encapsulate: Outer IPv6 DA = End.DT4 SID of PE-2
               SRH (may be empty if no TE, or with TE waypoints)
               Inner: original IPv4 packet from CE-1

P-nodes (transit):
  Route on outer IPv6 DA (PE-2's locator prefix)
  No MPLS, no VPN label awareness needed

PE-2 (egress):
  Receives packet, outer DA matches local End.DT4 SID
  Decapsulate, remove SRH, lookup inner IPv4 in VRF
  Forward to CE-2

Equivalent to: MPLS L3VPN with VPN label + transport label
               But: no LDP, no RSVP, no MPLS labels at all
```

### BGP EVPN over SRv6

SRv6 EVPN provides L2VPN and L2/L3 services:

- **End.DX2**: Decap + L2 cross-connect (EVPN VPWS)
- **End.DX2V**: Decap + VLAN-based L2 cross-connect
- **End.DT2M**: Decap + L2 table multicast (BUM traffic)

---

## How Datum Uses SRv6

This is where Datum's architecture diverges from traditional telco/5G SRv6 deployments. Datum uses SRv6 for a fundamentally different use case:

### SRv6 as Internet Backbone Overlay (Not 5G Transport)

```
Traditional SRv6 Deployment (Telco):
  5G RAN → 5G Core → SP Backbone (SRv6) → Internet
  Focus: replacing MPLS in carrier infrastructure

Datum's SRv6 Deployment (Open Network Cloud):
  Alt Cloud / Agent → Datum PoP → SRv6 Backbone → Datum PoP → Destination
  Focus: deterministic "fast lanes" across the internet for any application
```

### What Datum's SRv6 Backbone Provides

| Capability | How SRv6 Enables It |
|---|---|
| **Deterministic routing** | SR Policies define explicit paths between PoPs — traffic doesn't take unpredictable internet routes |
| **VPC-style isolation** | SRv6 VPN services (End.DT4/DT6) provide tenant isolation without MPLS |
| **Observability** | SRv6 paths are auditable — you know exactly which nodes traffic traverses |
| **Performance optimization** | End.X SIDs steer traffic through low-latency paths |
| **Multi-tenant** | Different SR Policies per customer/agent — each gets their own "fast lane" |
| **Programmable** | Kubernetes controllers can dynamically create/modify SR Policies |
| **No per-flow state** | Stateless forwarding scales to millions of flows without state explosion |

### The Controller-Driven Model

Unlike traditional SP deployments where SR Policies are manually configured or PCE-computed, Datum's approach is **controller-driven** via the Kubernetes control plane:

```
Kubernetes Control Plane
    │
    ├── Customer creates "Galactic VPC" resource (CRD)
    ├── Controller reconciles: determines required PoP-to-PoP paths
    ├── Computes SR Policies based on latency/topology constraints
    ├── Programs SRv6 data plane at each PoP
    └── Continuously monitors and adjusts (reconciliation loop)
```

---

## SRv6 Real-World Deployment Use Cases

### Service Provider Core
- **Replace MPLS transport**: SRv6 locators in IS-IS replace LDP labels
- **TE without RSVP**: End.X SIDs + SR Policy replace RSVP-TE tunnels
- **Seamless migration**: SRv6/MPLS interworking (End.BM) for phased migration
- **Multi-domain TE**: BGP SR Policy across AS boundaries

### Data Center Fabric
- **Replace VXLAN BGP EVPN underlay**: SRv6 for DC EVPN services
- **Service chaining (SFC)**: SRv6 encodes service function chains — each SID = a service function node
- **AI fabric optimization**: SRv6 uSID for deterministic RDMA traffic placement (emerging, Cisco active)

### Open Network Cloud (Datum's Pattern)
- **Internet backbone overlay**: SRv6 between global PoPs for deterministic routing
- **Virtual private backbones in minutes**: Controller-provisioned SR Policies via CRDs
- **Tenant isolation**: SRv6 VPN services per customer without MPLS infrastructure
- **Multi-cloud connectivity**: SRv6 paths across cloud providers via Datum PoPs
- **Agent-addressable networking**: Each AI agent can have its own SR Policy / "fast lane"

### Traditional Deployments
- **NTT Communications**: Full SRv6 backbone (Japan)
- **SoftBank**: SRv6 for 5G transport
- **China Mobile, China Telecom**: SRv6 backbone rollouts
- **Alibaba, Tencent**: SRv6 in DC fabrics
- **LinkedIn**: SRv6 in DC for service chaining and TE

---

# Datum's Full-Stack Architecture

## End-to-End Traffic Flow

```
AI Agent / Alt Cloud App / Developer Service
    │
    │  QUIC tunnel (Iroh) — P2P encrypted, NAT-traversing
    │  Identity: Ed25519 NodeId (zero trust, certificate-based)
    │
[Datum Edge PoP]
    │
    │  Envoy proxy: TLS termination, WAF (Coraza), AI Gateway routing
    │  HickoryDNS: Authoritative DNS resolution (17 locations)
    │  Metrics: OTel export to Grafana Cloud
    │
[Datum SRv6 Backbone]
    │
    │  SR Policy: deterministic path through global PoP mesh
    │  End.X SIDs for TE, End.DT4/6 for VPN isolation
    │  uSID compression for overhead efficiency
    │  Spin bit monitoring for per-flow RTT telemetry
    │
[Destination Datum PoP]
    │
    │  SRv6 decapsulation → VRF lookup → egress
    │  Envoy proxy: reverse proxy to backend service
    │
Destination Service (any cloud, bare metal, on-prem)
```

---

## The Operational Tension: SRv6 + QUIC Encryption

A critical architectural consideration that demonstrates deep understanding:

**Problem**: SRv6 service chaining can steer QUIC traffic through security appliances (firewall → IDS → LB), but QUIC encryption means **DPI appliances in the chain cannot inspect payload**. The chain must handle flows at the connection level, not packet level.

**Implications for Datum**:
- The SRv6 backbone can route and engineer QUIC tunnel traffic deterministically
- But the backbone is intentionally "blind" to tunnel contents (which is a *feature* for zero trust, not a bug)
- Observability relies on: spin bit RTT, flow-level telemetry (IPFIX), connection IDs, byte volumes, flow durations
- Security enforcement happens at the Envoy edge (WAF, bot detection, auth) rather than inline DPI in the backbone

This is a deliberate design philosophy: **security at the edge, performance in the backbone**.

---

## Envoy — The Edge Proxy

**Envoy** is a CNCF-graduated, high-performance C++ proxy used by Datum as the global edge layer:

| Role | Implementation |
|---|---|
| TLS termination | Envoy listener with TLS context |
| HTTP routing | HTTPProxy resources (Datum CRD) |
| WAF | Coraza (open-source, OWASP Core Rule Set) |
| AI traffic routing | Envoy AI Gateway (routes to GenAI backends) |
| Health checks | Built into Envoy |
| Metrics | Full OTel export to Grafana Cloud |
| Auth | Optional, configurable per-route |

Deployed across 17+ global PoPs. The AI Gateway extension is particularly relevant — it handles routing from application clients to Generative AI services, which is a key Datum use case.

---

## HickoryDNS

Datum uses **HickoryDNS**, a Rust-based DNS client, server, and resolver built for safety and security. Served across 17 global locations as an authoritative DNS service. Rust implementation = memory-safe, no buffer overflow vulnerabilities that have historically plagued DNS infrastructure.

---

## Crossplane

**Crossplane** is Datum's framework for normalizing cloud provider integrations:

```
Datum Kubernetes Control Plane
    │
    └── Crossplane Controllers
        │
        ├── AWS Provider → provisions AWS resources via CRDs
        ├── Azure Provider → provisions Azure resources via CRDs
        ├── GCP Provider → provisions GCP resources via CRDs
        └── Custom Providers → bare metal, edge locations
```

Key value: Datum's customers deploy across many clouds (the "alt cloud" ecosystem). Crossplane provides a **single, declarative API** to manage infrastructure across all of them, with continuous reconciliation ensuring desired state = actual state.

---

## Galactic VPCs

A planned Datum feature: **Galactic VPCs** — virtual private clouds that span multiple clouds and data centers. This maps directly to SRv6 L3VPN capabilities:

- Each Galactic VPC = an SRv6 VPN with tenant-specific End.DT4/DT6 SIDs
- Spans across Datum's global PoPs
- Unified network management across hybrid/multi-cloud
- Real-time streaming telemetry
- Defined as Kubernetes CRDs — `kubectl apply` a VPC into existence

---

# Interview Talking Points

## Key Concepts to Demonstrate

### 1. You Understand Datum's "Why"
The internet is fragmenting (sovereignty, regulation, alt-cloud explosion). Traditional private networks require dedicated teams and years to build. Datum makes SP-grade networking capabilities (peering, TE, private backbone, interconnection) available to every developer and AI agent via an open-source, API-first platform.

### 2. You Understand the Three-Layer Architecture
- **Edge**: Envoy + Coraza WAF + AI Gateway (17+ PoPs)
- **Tunnels**: Iroh (QUIC P2P, hole punching, relay fallback, zero trust)
- **Backbone**: SRv6 overlay (deterministic routing, VPC isolation, telemetry)

### 3. You Can Explain How SRv6 Differs from Traditional SP Use
Datum uses SRv6 not for 5G transport but for **internet backbone programmability** — an open-source network cloud where SR Policies are controller-driven via Kubernetes CRDs, not manually provisioned or PCE-computed carrier tunnels.

### 4. You Know Why QUIC (Not TCP) for Tunnels
Connection migration (survives network changes), built-in encryption (relay can't inspect), stream multiplexing (independent flows), userspace implementation (rapid iteration), NAT traversal compatibility (UDP-based).

### 5. You Can Articulate the SRv6 + QUIC Tension
SRv6 provides deterministic routing and service chaining, but QUIC encryption makes inline DPI impossible. Datum's design philosophy: security at the edge (Envoy/WAF), performance in the backbone (SRv6 TE). The backbone is intentionally blind to payload — zero trust.

### 6. You Understand the Control Plane
Kubernetes CRDs + Crossplane for multi-cloud normalization. Reconciliation loops. GitOps-compatible. LLM-friendly API surface. KEP-4080 for generic control planes without container orchestration semantics.

### 7. You Know the Team's Lineage
Founded by Packet/Equinix/Voxel/Fastly/StackPath veterans. These are people who built bare-metal cloud automation, ran global data center platforms, and deeply understand peering, interconnection, and SP-grade networking.

### 8. Industry Context — SRv6 Momentum Beyond Telco
SRv6 is expanding from 5G/SP deployments into AI infrastructure (GPU fabric optimization, RDMA traffic placement, cross-DC scale-across architectures). Cisco is actively promoting SRv6 uSID for AI workloads. Datum is part of this broader trend of SRv6 moving into non-traditional use cases.

---

# Key RFCs & References

## QUIC
| RFC | Title |
|---|---|
| RFC 8999 | Version-Independent Properties of QUIC |
| RFC 9000 | QUIC: A UDP-Based Multiplexed and Secure Transport (**core spec**) |
| RFC 9001 | Using TLS to Secure QUIC |
| RFC 9002 | QUIC Loss Detection and Congestion Control |
| RFC 9221 | Unreliable Datagram Extension to QUIC (DATAGRAM frames) |
| RFC 9250 | DNS over Dedicated QUIC Connections (DoQ) |
| RFC 9369 | QUIC Version 2 |
| RFC 9484 | Proxying IP in HTTP (CONNECT-IP) |
| draft-ietf-quic-multipath | QUIC Multipath (active draft — relevant to Iroh) |

## MASQUE
| RFC | Title |
|---|---|
| RFC 9297 | HTTP Datagrams and the Capsule Protocol |
| RFC 9298 | Proxying UDP in HTTP (CONNECT-UDP) |
| RFC 9484 | Proxying IP in HTTP (CONNECT-IP) |
| RFC 9412 | The MASQUE Problem Statement |

## SRv6
| RFC | Title |
|---|---|
| RFC 8402 | Segment Routing Architecture |
| RFC 8754 | IPv6 Segment Routing Header (SRH) |
| RFC 8814 | Signaling MSD using BGP-LS |
| RFC 8986 | SRv6 Network Programming (**core endpoint behaviors**) |
| RFC 9252 | BGP Overlay Services Based on SRv6 (L3VPN, EVPN) |
| RFC 9259 | OAM for SRv6 |
| draft-ietf-spring-srv6-srh-compression | uSID / SRv6 compression (active draft) |

## Datum-Specific References
| Reference | Description |
|---|---|
| [datum.net](https://datum.net) | Datum Cloud — product, docs, blog |
| [github.com/datum-cloud](https://github.com/datum-cloud) | Open source repos (AGPLv3) |
| [datum.net/blog/every-agent-needs-an-edge](https://datum.net/blog/every-agent-needs-an-edge/) | "Splinternet" thesis and architecture overview (Feb 2026) |
| [datum.net/blog/control-plane-for-modern-service-providers](https://datum.net/blog/control-plane-for-modern-service-providers/) | K8s control plane design rationale |
| [docs.iroh.computer](https://docs.iroh.computer) | Iroh documentation (QUIC connectivity library) |
| [iroh.computer/blog/iroh-on-QUIC-multipath](https://iroh.computer/blog/iroh-on-QUIC-multipath) | QUIC Multipath work in Quinn/Iroh |
| [crossplane.io](https://crossplane.io) | Crossplane — cloud-native control plane framework |

## General
| Reference | Description |
|---|---|
| [quicwg.org](https://quicwg.org) | IETF QUIC Working Group |
| [segment-routing.net](https://segment-routing.net) | SRv6/SR-MPLS resources |
| Wireshark | Full QUIC and SRv6 dissectors (v3.3+) |
| Quinn | Pure-Rust QUIC implementation (basis of Iroh) |
| FRRouting (FRR) | Open-source routing suite with SRv6 support |
| VPP (fd.io) | High-performance dataplane with SRv6 and QUIC support |
