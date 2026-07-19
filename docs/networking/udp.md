# User Datagram Protocol

> **Applies to:** General UDP concepts for IPv4 and IPv6 networks
> **Last reviewed:** 2026-07-18

User Datagram Protocol (UDP) provides connectionless datagram delivery with low protocol overhead. UDP does not establish a session or guarantee delivery, ordering, duplicate suppression, congestion control, or retransmission.

## Datagram model

Each UDP datagram is an independent message containing:

| Field | Purpose |
|---|---|
| Source port | Optional sender application port |
| Destination port | Receiver application port |
| Length | UDP header plus payload length |
| Checksum | Integrity check covering header, payload, and IP pseudo-header |

Applications using UDP must tolerate loss, duplication, reordering, or implement recovery behavior themselves.

## Common uses

- DNS queries and responses;
- DHCP;
- voice and video media streams;
- telemetry and monitoring;
- gaming and real-time applications;
- VPN and tunneling protocols;
- QUIC, which implements reliable transport behavior above UDP.

## Read-only inspection

Linux examples:

```bash
ss -uanp
ss -s
ip -s link
nstat
```

Packet capture examples:

```bash
tcpdump -ni <interface> 'udp and host <ip-address>'
tcpdump -ni <interface> 'udp port <port>'
```

## Operational considerations

- A missing response does not reveal whether the request, response, or application failed.
- Stateful firewalls and NAT devices maintain time-limited UDP flow entries even though UDP itself is stateless.
- Large datagrams may fragment or fail when the path MTU is too small.
- High-rate UDP applications must implement appropriate pacing and congestion behavior.
- ICMP errors can provide useful evidence but may be filtered or rate-limited.

## Common symptoms

| Symptom | Likely checks |
|---|---|
| No response | Service listener, firewall, routing, NAT, application timeout |
| Intermittent loss | Congestion, interface drops, buffers, wireless quality, policing |
| Works for small messages only | Path MTU, fragmentation, tunnel overhead |
| One-way audio or media | NAT, firewall pinholes, asymmetric routing, advertised addresses |
| Duplicate or out-of-order data | Application sequencing and loss handling |

## Troubleshooting workflow

1. Confirm the destination address and UDP port.
2. Verify the service is listening and bound to the intended interface.
3. Capture traffic at the sender and receiver.
4. Check routing, firewall, NAT, and load-balancer state in both directions.
5. Compare packet counters, drops, and application logs.
6. Test payload sizes to identify MTU or fragmentation behavior.
7. Validate application-level retry, sequencing, and timeout logic.

## References

- [RFC 768: User Datagram Protocol](https://www.rfc-editor.org/rfc/rfc768)
