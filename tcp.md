### TCP (Transmission Control Protocol) Cheat Sheet

#### Introduction to TCP


TCP is a connection-oriented protocol used for reliable transmission of data over a network. It ensures that data is delivered accurately and in the same order as it was sent.

- **Function**: Reliable, ordered, and error-checked delivery of a stream of data between applications.
- **Use Cases**: Web browsing, email, file transfers, and other situations where data integrity is crucial.


#### Key Features of TCP


- **Connection-Oriented**: Establishes a connection before transmitting data.
- **Reliability**: Resends data if it's lost during transmission.
- **Flow Control**: Manages data transmission rate to prevent network congestion.
- **Congestion Control**: Adjusts data transfer rate based on network conditions.
- **Ordered Delivery**: Ensures data is received in the order it was sent.
- **Error Checking**: Uses checksums to verify data integrity.


#### TCP Operations


- **3-Way Handshake**: Process of establishing a TCP connection (SYN, SYN-ACK, ACK).
- **Data Transfer**: After connection establishment, data is exchanged between sender and receiver.
- **Connection Termination**: Connections are terminated using a FIN flag, followed by an acknowledgment.


#### TCP Header


- **Source and Destination Ports**: Identify sending and receiving applications.
- **Sequence and Acknowledgment Numbers**: Manage data ordering and delivery confirmation.
- **Flags**: Control the state of the connection (SYN, ACK, FIN, RST, etc.).
- **Window Size**: Used for flow control.
- **Checksum**: Ensures data integrity.


#### Troubleshooting TCP


- **Network Analysis Tools**: Use tools like Wireshark to analyze TCP traffic and troubleshoot issues.
- **Common Issues**: Lost connections, retransmissions, and timeouts.
