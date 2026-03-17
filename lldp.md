# LLDP (Link Layer Discovery Protocol) Cheat Sheet

## Introduction to LLDP

LLDP (Link Layer Discovery Protocol) is a standardized network discovery protocol used in local networks. It allows a network device to advertise its identity and capabilities to neighboring devices and receive similar information from them. This protocol is crucial in complex networks for documenting and troubleshooting network topologies.

- **Function**: Device discovery and identification in a network.
- **Use Cases**: Network mapping, troubleshooting, and ensuring correct network configurations.

## Layman's Explanation of LLDP

Think of LLDP as a 'name tag' system for network devices. Just like people wearing name tags at a conference to identify themselves, network devices (like switches and routers) use LLDP to announce who they are, where they are, and what they can do. This makes it easier to understand how the network is set up and how devices are connected and interacting with each other.

## Key Components of LLDP

- **LLDP Agent**: Software running on network devices that sends and receives LLDP frames.
- **LLDP Frame**: Contains information about the device, such as device ID, port ID, and capabilities.
- **Management Address**: An address used to manage the device.
- **TLVs (Type-Length-Value)**: The format in which LLDP information is transmitted. Includes details like system name, system description, port description, etc.

## Basic LLDP Operations

- **Enabling LLDP**
  - LLDP is often enabled by default on capable devices. It can be turned on/off in the device's settings.

- **Viewing LLDP Information**
  - Most network devices allow you to view LLDP information via their CLI or GUI. Commands vary by device.
  - Example (Cisco CLI): `show lldp neighbors`
  - Example (Juniper CLI): `show lldp neighbors`
  - Example (Arista CLI): `show lldp neighbors`

## Configuring LLDP

- **Cisco IOS / IOS-XE**
  - Enable LLDP globally: `lldp run`
  - Enable LLDP on an interface:
    ```
    interface [interface-name]
     lldp transmit
     lldp receive
    ```
  - Disable LLDP on an interface:
    ```
    interface [interface-name]
     no lldp transmit
     no lldp receive
    ```
  - Set LLDP timer: `lldp timer [seconds]`
  - Set LLDP holdtime: `lldp holdtime [seconds]`

- **Juniper Junos**
  - Enable LLDP:
    ```
    set protocols lldp interface all
    ```
  - Enable LLDP on a specific interface:
    ```
    set protocols lldp interface [interface-name]
    ```
  - Disable LLDP on a specific interface:
    ```
    set protocols lldp interface [interface-name] disable
    ```

- **Arista EOS**
  - Enable LLDP globally: `lldp run`
  - Disable LLDP on an interface:
    ```
    interface [interface-name]
     no lldp transmit
     no lldp receive
    ```

## Customizing TLVs

- You can configure what information is included in the LLDP frames.
- Example (Cisco): Disable specific TLVs on an interface:
  ```
  interface [interface-name]
   no lldp tlv-select port-description
   no lldp tlv-select system-capabilities
  ```

## Useful Show Commands

| Platform | Command | Description |
|---|---|---|
| Cisco | `show lldp` | Displays global LLDP status |
| Cisco | `show lldp neighbors` | Lists LLDP neighbors |
| Cisco | `show lldp neighbors detail` | Shows detailed neighbor info |
| Cisco | `show lldp interface [interface]` | Shows LLDP status on an interface |
| Cisco | `show lldp traffic` | Displays LLDP traffic statistics |
| Juniper | `show lldp` | Displays LLDP configuration |
| Juniper | `show lldp neighbors` | Lists LLDP neighbors |
| Juniper | `show lldp statistics` | Displays LLDP statistics |
| Arista | `show lldp neighbors` | Lists LLDP neighbors |
| Arista | `show lldp neighbors detail` | Shows detailed neighbor info |

## LLDP in Network Management

- **Network Mapping**: LLDP helps in automatically mapping the network, identifying how devices are interconnected.
- **Troubleshooting**: Quickly identify misconfigurations or connectivity issues in the network.
- **Inventory Management**: Keep track of devices and their capabilities in large networks.
- **NMS Integration**: Many Network Management Systems (NMS) use LLDP data to automatically build topology maps.

## Security Considerations

- **Unauthorized Access**: As LLDP can reveal network topology, it is important to secure access to LLDP information.
- **Disabling on Untrusted Ports**: It is a common practice to disable LLDP on ports facing untrusted networks, customers, or the public internet to avoid exposing internal topology details.
- **Edge Ports**: Always disable LLDP transmit on end-user facing ports (access ports connected to workstations, printers, etc.) to minimize information leakage.
- **LLDP-MED**: LLDP Media Endpoint Discovery (LLDP-MED) is an extension used for VoIP and other media endpoints. Ensure it is only enabled where needed.

## Tips for Using LLDP

- **Documentation**: Use LLDP neighbor data to keep network topology documentation up to date automatically.
- **Verify Cabling**: Mismatched LLDP neighbor information is often a sign of incorrect cabling or port assignments.
- **Combine with CDP**: On Cisco networks, LLDP and CDP can run simultaneously to support both Cisco-only and multi-vendor environments.
