# OSPF (Open Shortest Path First) Cheat Sheet

## Introduction to OSPF

OSPF (Open Shortest Path First) is a widely used interior gateway protocol (IGP) in large enterprise networks. It's a link-state routing protocol that uses a link-state database to calculate the shortest path for data packets within an Autonomous System (AS).

- **Function**: Dynamic routing within a single routing domain.
- **Use Cases**: Efficient routing in large and complex networks, load balancing, and redundancy.

## Layman's Explanation of OSPF

Imagine OSPF as a GPS system for network data packets. Just like a GPS finds the best route for a car based on current traffic and road conditions, OSPF finds the most efficient path for data to travel across a network. It constantly updates its map of the network (link-state database) to ensure data always takes the quickest route, avoiding any 'traffic jams' or 'road closures' in the network.

## Key Components of OSPF

- **Router ID**: A unique identifier for each router participating in OSPF. Typically the highest IP address on a loopback interface.
- **Area**: A logical grouping of networks and routers. OSPF calculations are performed within an area. Area 0 is the backbone area.
- **Link-State Advertisement (LSA)**: OSPF messages that contain information about neighbors and network topology.
- **Link-State Database (LSDB)**: A database containing all LSAs. Each OSPF router within the same area maintains an identical copy.
- **Cost**: A metric used by OSPF to determine the shortest path. Cost is typically derived from interface bandwidth. Lower cost is preferred.
- **Designated Router (DR)**: On multi-access networks, the DR reduces LSA flooding by acting as a central point for LSA exchange.
- **Backup Designated Router (BDR)**: Standby to the DR, takes over if the DR fails.

## OSPF Area Types

| Area Type | Description |
|---|---|
| Backbone (Area 0) | All areas must connect to Area 0. The core of the OSPF domain. |
| Standard Area | Receives all LSA types including external routes. |
| Stub Area | Does not accept external LSAs. Uses a default route instead. |
| Totally Stubby Area | Cisco proprietary. Accepts only a default route — no inter-area or external LSAs. |
| Not-So-Stubby Area (NSSA) | Allows external routes via Type 7 LSAs, converted to Type 5 at the ABR. |

## Basic OSPF Operations

- **Enabling OSPF (Cisco IOS)**
  ```
  router ospf [process-id]
  router-id [A.B.C.D]
  network [network-address] [wildcard-mask] area [area-id]
  ```

- **Enabling OSPF on an Interface (Cisco IOS)**
  ```
  interface [interface-name]
   ip ospf [process-id] area [area-id]
  ```

- **Enabling OSPF (Juniper Junos)**
  ```
  set protocols ospf area [area-id] interface [interface-name]
  ```

- **Enabling OSPF (Arista EOS)**
  ```
  router ospf [process-id]
   network [network-address/prefix-length] area [area-id]
  ```

## Configuring OSPF

- **OSPF Network Types**
  - `broadcast` — Default for Ethernet. Elects DR/BDR.
  - `point-to-point` — No DR/BDR election. Common on WAN links.
  - `non-broadcast` (NBMA) — Manually configure neighbors.
  - Change network type (Cisco): `ip ospf network [type]`

- **OSPF Timers**
  - Hello interval (default 10s on broadcast, 30s on NBMA):
    `ip ospf hello-interval [seconds]`
  - Dead interval (default 4× hello interval):
    `ip ospf dead-interval [seconds]`
  - Note: Hello and dead intervals must match between OSPF neighbors.

- **Route Summarization (at ABR)**
  ```
  router ospf [process-id]
   area [area-id] range [network] [mask]
  ```

- **Redistribute into OSPF**
  ```
  router ospf [process-id]
   redistribute [protocol] subnets
  ```

- **Default Route into OSPF**
  ```
  router ospf [process-id]
   default-information originate [always]
  ```

## Useful Show Commands

| Command | Description |
|---|---|
| `show ip ospf neighbor` | Displays OSPF neighbor relationships and states |
| `show ip ospf neighbor detail` | Shows detailed neighbor information |
| `show ip ospf interface` | Shows OSPF-related info on interfaces including DR/BDR |
| `show ip ospf database` | Displays the OSPF link-state database (LSDB) |
| `show ip ospf database router` | Shows router LSAs (Type 1) |
| `show ip ospf database network` | Shows network LSAs (Type 2) |
| `show ip ospf database summary` | Shows summary LSAs (Type 3) |
| `show ip ospf database external` | Shows external LSAs (Type 5) |
| `show ip route ospf` | Shows routes learned via OSPF |
| `show ip ospf` | Shows OSPF process information |
| `show ip ospf traffic` | Displays OSPF traffic statistics |
| `show ip ospf border-routers` | Shows ABRs and ASBRs known to OSPF |
| `show ip ospf virtual-links` | Displays OSPF virtual link configurations |
| `debug ip ospf events` | Enables OSPF event debugging |
| `debug ip ospf adj` | Debugs OSPF adjacency formation |

## OSPF Neighbor States

| State | Description |
|---|---|
| Down | No hellos received from neighbor |
| Attempt | Unicast hellos being sent (NBMA only) |
| Init | Hello received, but router's own RID not seen in neighbor's hello |
| 2-Way | Bidirectional communication established; DR/BDR election happens here |
| ExStart | Master/slave relationship established for DBD exchange |
| Exchange | Database Description (DBD) packets being exchanged |
| Loading | LSR/LSU packets being exchanged to synchronize LSDB |
| Full | LSDB fully synchronized; normal operation |

## OSPF in Network Management

- **Scalability**: OSPF supports large networks with hierarchical design using areas. Use multiple areas to limit LSDB size and LSA flooding scope.
- **Fast Convergence**: Quickly recalculates routes when network changes occur using the Dijkstra (SPF) algorithm.
- **Load Balancing**: Supports up to 16 equal-cost paths (ECMP) for load balancing (4 by default on Cisco).

## Security Considerations

- **Authentication**: OSPF supports multiple authentication types to secure routing information:
  - None (default — not recommended in production)
  - Simple (plaintext password — weak)
  - MD5 (cryptographic hash — recommended)
  - SHA (supported in OSPFv3 for IPv6)
  - Configure MD5 (Cisco):
    ```
    interface [interface-name]
     ip ospf authentication message-digest
     ip ospf message-digest-key [key-id] md5 [password]
    ```
- **Route Filtering**: Controls route advertisement and prevents the propagation of incorrect routing information using distribute-lists or prefix-lists.
  ```
  router ospf [process-id]
   distribute-list prefix [prefix-list-name] in
  ```
- **Passive Interfaces**: Suppress OSPF hellos on interfaces that should not form adjacencies (e.g., end-user facing ports):
  ```
  router ospf [process-id]
   passive-interface [interface-name]
  ```
