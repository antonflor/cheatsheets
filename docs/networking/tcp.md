# Transmission Control Protocol

> **Applies to:** General TCP concepts for IPv4 and IPv6 networks
> **Last reviewed:** 2026-07-18

Transmission Control Protocol (TCP) provides a connection-oriented byte stream with ordered delivery, retransmission, flow control, and congestion control. Applications use TCP ports to identify services at each endpoint.

## Connection lifecycle

A connection normally begins with the three-way handshake:

```text
client -> server  SYN
server -> client  SYN, ACK
client -> server  ACK
```

Normal close commonly exchanges FIN and ACK segments in each direction. A reset (`RST`) terminates a connection immediately and usually indicates rejection, an invalid connection state, or an application or firewall action.

## Important header fields

| Field | Purpose |
|---|---|
| Source and destination ports | Identify endpoint applications |
| Sequence number | Identifies byte position in the stream |
| Acknowledgment number | Confirms received bytes |
| Flags | SYN, ACK, FIN, RST, PSH, URG, ECE, CWR |
| Window | Advertises receive capacity for flow control |
| Checksum | Detects corruption across header and payload |
| Options | MSS, window scaling, timestamps, selective acknowledgment |

## Reliability behavior

- Missing data is retransmitted.
- Duplicate acknowledgments can indicate loss or reordering.
- Selective acknowledgment allows a receiver to describe noncontiguous data already received.
- Receive-window limits provide endpoint flow control.
- Congestion-control algorithms adjust sending behavior based on network conditions.

TCP guarantees ordered delivery to the application, but it does not preserve application message boundaries. Applications must define their own framing.

## Read-only inspection

Linux examples:

```bash
ss -tanp
ss -s
ip -s link
nstat
```

Packet capture examples:

```bash
tcpdump -ni <interface> 'tcp and host <ip-address>'
tcpdump -ni <interface> 'tcp port <port>'
```

## Common symptoms

| Symptom | Likely checks |
|---|---|
| SYN retransmissions | Routing, firewall, listener, asymmetric path, packet loss |
| Immediate RST | Closed port, application rejection, firewall or load balancer reset |
| Repeated retransmissions | Loss, congestion, bad link, MTU or path issue |
| Zero window | Receiver or application cannot consume data quickly enough |
| Slow single flow | Congestion window, latency, loss, receive window, application behavior |
| Connection stalls after handshake | TLS or application protocol, MTU, middlebox inspection |

## Troubleshooting workflow

1. Confirm name resolution and destination address.
2. Confirm the server is listening on the expected address and port.
3. Verify routing and firewall policy in both directions.
4. Capture the handshake and identify which side stops responding.
5. Check retransmissions, resets, advertised windows, MSS, and timestamps.
6. Compare application logs with packet timestamps.
7. Test from another source or path before changing production settings.

## References

- [RFC 9293: Transmission Control Protocol](https://www.rfc-editor.org/rfc/rfc9293)
