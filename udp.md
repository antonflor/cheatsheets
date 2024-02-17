### UDP (User Datagram Protocol) Cheat Sheet

#### Introduction to UDP


UDP is a connectionless protocol used for fast transmission of data over a network. It's lightweight and efficient but does not guarantee reliable or ordered delivery of data.

- **Function**: Transmits data quickly with minimal overhead, but without guarantees of reliability or order.
- **Use Cases**: Streaming media, online gaming, VoIP, and other time-sensitive applications.


#### Key Features of UDP


- **Connectionless**: No connection establishment is required.
- **Speed**: Faster data transmission due to minimal protocol overhead.
- **No Congestion Control**: Does not adjust for network congestion.
- **Unreliable Delivery**: No guarantee of packet delivery.
- **No Ordering**: Data may arrive out of order.


#### UDP Operations


- **Datagram Transmission**: Sends data in discrete packets called datagrams.
- **No Acknowledgment**: Does not confirm receipt of data.
- **Stateless**: Each datagram is independent of others.


#### UDP Header


- **Source and Destination Ports**: Identify sending and receiving applications.
- **Length**: Specifies the length of the UDP header and data.
- **Checksum**: Optional field for error checking.


#### Troubleshooting UDP


- **Packet Loss**: Common in congested networks; important to monitor in applications using UDP.
- **Application-Level Reliability**: Applications may need to implement their own reliability mechanisms.
