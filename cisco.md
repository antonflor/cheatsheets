# Cisco Command Cheat Sheet

This document is a curated collection of essential Cisco commands for network and systems engineers. It focuses on troubleshooting Layer 2 issues and general network management on Cisco devices. These commands are vital for diagnosing connectivity problems, VLAN configurations, STP issues, and more.

The cheat sheet is designed as a quick reference to facilitate daily network management tasks and to assist in rapidly resolving common network issues. Please use these commands with caution, as they can significantly impact network operations, especially in a production environment.

## Interface Management and Status

- **show interfaces**
  - Displays status and statistics for all interfaces.

- **show interfaces [interface]**
  - Shows detailed information about a specific interface.

- **show interfaces status**
  - Provides a quick overview of all interfaces' status.

- **show interface [INTERFACE_NAME] switchport**
  - Displays switchport information for a specific interface.

- **show interface [INTERFACE_NAME] status**
  - Shows the status of a specific interface.

- **show interface [INTERFACE_NAME] counters**
  - Shows detailed counters for a specific interface.

- **show interface counters errors**
  - Displays interface error counters.

- **show interfaces description**
  - Displays interface descriptions and status.

- **test cable-diagnostics tdr interface [INTERFACE_NAME]**
  - Runs a Time Domain Reflectometer (TDR) test on a specified interface.


## VLAN and Trunking

- **show vlan**
  - Displays VLAN information.

- **show vlan brief**
  - Provides a brief summary of all VLANs.

- **vlan [VLAN_ID]**
  - Creates a VLAN or enters VLAN configuration mode.

- **show interfaces trunk**
  - Displays trunk ports and their encapsulation.

- **switchport mode access**
  - Sets an interface to access mode.

- **switchport mode trunk**
  - Sets an interface to trunk mode.

- **switchport access vlan [VLAN_ID]**
  - Assigns an access VLAN to an interface.

- **switchport trunk allowed vlan [VLAN_LIST]**
  - Specifies VLANs allowed on a trunk interface.

## Spanning Tree Protocol (STP)

- **show spanning-tree**
  - Displays Spanning Tree Protocol (STP) information.

- **show spanning-tree summary**
  - Shows a summary of STP status.

- **show spanning-tree root**
  - Displays the root bridge information for STP.

- **show spanning-tree [vlan VLAN_ID]**
  - Shows STP information for a specific VLAN.

- **debug spanning-tree events**
  - Enables debugging for STP events.

## EtherChannel and Port Security

- **show etherchannel summary**
  - Provides a summary of EtherChannel status.

- **channel-group [NUMBER] mode active**
  - Configures an interface to actively form an EtherChannel.

- **show port-security**
  - Displays port security settings.

- **show port-security interface [interface]**
  - Shows port security details on a specific interface.

## Discovery Protocols and Environmental Information

- **show cdp neighbors**
  - Lists Cisco Discovery Protocol (CDP) neighbor devices.

- **show cdp neighbors detail**
  - Provides detailed information about CDP neighbors.

- **show lldp neighbors**
  - Lists Link Layer Discovery Protocol (LLDP) neighbor devices.

- **show lldp neighbors detail**
  - Provides detailed information about LLDP neighbors.

- **show environment**
  - Displays environmental information like temperature and power supply status.

## System Information and Configuration

- **show version**
  - Displays system hardware and software status.

- **show inventory**
  - Shows hardware inventory details.

- **show running-config interface [INTERFACE_NAME]**
  - Shows the running configuration of a specific interface.

- **show ip interface brief**
  - Summarizes IP interface status and configuration.

- **show processes cpu**
  - Shows CPU utilization.

- **show processes memory**
  - Displays memory usage.

- **show logging**
  - Displays the system log for potential error messages or alerts.

- **copy running-config startup-config**
  - Saves the current configuration to the startup configuration.

- **show boot**
  - Shows boot path and image information.

- **show flash:**
  - Displays the contents of the flash memory.

## MAC Address Table and ARP

- **show mac address-table**
  - Displays the MAC address table.

- **show mac address-table dynamic**
  - Shows dynamically learned MAC addresses.

- **show arp**
  - Shows the ARP table.

- **clear mac address-table dynamic**
  - Clears the dynamic entries from the MAC address table.

## Miscellaneous Commands

- **show vtp status**
  - Displays VLAN Trunking Protocol (VTP) status.

- **show udld [INTERFACE_NAME]**
  - Shows UniDirectional Link Detection (UDLD) status on an interface.

- **clear counters [interface]**
  - Clears the counters on a specific interface.

- **show sdm prefer**
  - Shows the Switch Database Management (SDM) template in use.

- **show standby**
  - Displays Hot Standby Router Protocol (HSRP) information.

- **show access-lists**
  - Displays configured access lists.

- **show power inline**
  - Shows Power over Ethernet (PoE) status on interfaces.

- **show redundancy**
  - Displays redundancy information (useful in MLAG setups).

- **show vrf**
  - Displays VRF (Virtual Routing and Forwarding) information.

- **show mlag**
  - Displays MLAG configuration and status.

- **mlag peer**
  - Configures MLAG peer settings.

- **mlag domain**
  - Configures an MLAG domain.

- **show mlag detail**
  - Provides detailed information about MLAG status.
