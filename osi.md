### OSI Model Cheat Sheet

#### Introduction to the OSI Model


The OSI (Open Systems Interconnection) model is a conceptual framework used to understand network interactions in seven distinct layers. Each layer serves a specific function and communicates with the layers directly above and below it.

- **Purpose**: Standardizes the functions of a telecommunication or computing system without regard to its underlying internal structure and technology.


#### Layer 1: Physical


- **Function**: Transmits raw bit streams over a physical medium.
- **Components**: Cables, fibers, physical network interfaces, hubs, repeaters.
- **Role**: Defines physical and electrical specifications of devices and media.


#### Layer 2: Data Link


- **Function**: Manages node-to-node data transfer and error handling.
- **Sublayers**: Media Access Control (MAC), Logical Link Control (LLC).
- **Components**: Switches, bridges, network interface cards (NICs).
- **Protocols**: Ethernet, PPP, Switching, ARP.


#### Layer 3: Network


- **Function**: Manages device addressing, tracks the location of devices, and determines the best way to move data.
- **Components**: Routers, layer 3 switches.
- **Protocols**: IP, ICMP, IPSec, IGMP, OSPF, BGP.


#### Layer 4: Transport


- **Function**: Provides reliable data transfer across a network.
- **Characteristics**: TCP (connection-oriented) and UDP (connectionless).
- **Role**: Manages end-to-end message delivery, error recovery, and flow control.


#### Layer 5: Session


- **Function**: Manages sessions or connections between applications.
- **Role**: Establishes, manages, and terminates connections between applications.
- **Protocols**: NetBIOS, SMB, RPC, PPTP.


#### Layer 6: Presentation


- **Function**: Translates data between the application layer and the network.
- **Role**: Data encryption, decryption, compression, and translation.
- **Examples**: SSL, TLS, MIME, ASCII, EBCDIC, JPEG, MPEG.


#### Layer 7: Application


- **Function**: Closest to the end user. Interacts with software applications that implement a communicating component.
- **Role**: HTTP, FTP, SMTP, DNS, Telnet, SNMP, and more.
- **Characteristics**: Serves as the window for users and application processes to access network services.


#### Understanding the OSI Model


- **Data Encapsulation**: Each layer adds its own header (or trailer) to the data as it passes through.
- **Data Flow**: Data flows down the layers on the sending end and up the layers on the receiving end.
- **Layer Interaction**: Each layer on the sending device communicates with its corresponding layer on the receiving device.


#### Practical Applications


- **Troubleshooting**: Helps in isolating network issues by layer.
- **Network Design**: Guides the design and understanding of network architectures.
- **Protocol Implementation**: Assists in understanding and implementing network protocols.
