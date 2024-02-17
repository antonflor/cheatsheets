### LLDP (Link Layer Discovery Protocol) Cheat Sheet

#### Introduction to LLDP

LLDP (Link Layer Discovery Protocol) is a standardized network discovery protocol used in local networks. It allows a network device to advertise its identity and capabilities to neighboring devices and receive similar information from them. This protocol is crucial in complex networks for documenting and troubleshooting network topologies.

- **Function**: Device discovery and identification in a network.
- **Use Cases**: Network mapping, troubleshooting, and ensuring correct network configurations.

#### Layman's Explanation of LLDP


Think of LLDP as a 'name tag' system for network devices. Just like people wearing name tags at a conference to identify themselves, network devices (like switches and routers) use LLDP to announce who they are, where they are, and what they can do. This makes it easier to understand how the network is set up and how devices are connected and interacting with each other.

#### Key Components of LLDP


- **LLDP Agent**: Software running on network devices that sends and receives LLDP frames.
- **LLDP Frame**: Contains information about the device, such as device ID, port ID, and capabilities.
- **Management Address**: An address used to manage the device.
- **TLVs (Type-Length-Value)**: The format in which LLDP information is transmitted. Includes details like system name, system description, port description, etc.

#### Basic LLDP Operations


- **Enabling LLDP**
  - LLDP is often enabled by default on capable devices. It can be turned on/off in the device's settings.

- **Viewing LLDP Information**
  - Most network devices allow you to view LLDP information via their CLI or GUI. Commands vary by device.
  - Example (Cisco CLI): `show lldp neighbors`

#### Configuring LLDP


- **Configuration Commands**
  - The specific commands to configure LLDP depend on the network device and its operating system.
  - Example (Cisco CLI):
    - Enable LLDP: `lldp run`
    - Configure LLDP settings on an interface: `interface [interface-name]` followed by `lldp transmit` and `lldp receive`

- **Customizing TLVs**
  - You can configure what information is included in the LLDP frames.

#### LLDP in Network Management


- **Network Mapping**: LLDP helps in automatically mapping the network, identifying how devices are interconnected.
- **Troubleshooting**: Quickly identify misconfigurations or connectivity issues in the network.
- **Inventory Management**: Keep track of devices and their capabilities in large networks.

#### Security Considerations


- **Unauthorized Access**: As LLDP can reveal network topology, it's important to secure access to LLDP information.
- **Disabling on Untrusted Ports**: It's a common practice to disable LLDP on ports facin
