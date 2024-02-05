# Cisco Command Cheat Sheet

This document is a curated collection of essential Cisco commands for network and systems engineers. It focuses on troubleshooting Layer 2 issues and general network management on Cisco devices. These commands are vital for diagnosing connectivity problems, VLAN configurations, STP issues, and more.

The cheat sheet is designed as a quick reference to facilitate daily network management tasks and to assist in rapidly resolving common network issues. Please use these commands with caution, as they can significantly impact network operations, especially in a production environment.

---

**show interfaces**
- Displays status and statistics for all interfaces.

**show interfaces [interface]**
- Shows detailed information about a specific interface.

**show interfaces status**
- Provides a quick overview of all interfaces' status.

**show mac address-table**
- Displays the MAC address table.

**show mac address-table dynamic**
- Shows dynamically learned MAC addresses.

**show vlan**
- Displays VLAN information.

**show vlan brief**
- Provides a brief summary of all VLANs.

**show spanning-tree**
- Displays Spanning Tree Protocol (STP) information.

**show spanning-tree summary**
- Shows a summary of STP status.

**show spanning-tree root**
- Displays the root bridge information for STP.

**show vlan**
  - Shows VLAN configurations and status.

**show interfaces trunk**
  - Displays trunk interfaces and their status.

**show etherchannel summary**
  - Provides a summary of EtherChannel status.

**show spanning-tree**
  - Displays Spanning Tree Protocol (STP) information.

**show interfaces switchport**
  - Shows the switchport configuration of an interface.

**show vtp status**
  - Displays VLAN Trunking Protocol (VTP) status.

**show cdp neighbors**
  - Lists Cisco Discovery Protocol (CDP) neighbor devices.

**show lldp neighbors**
  - Shows Link Layer Discovery Protocol (LLDP) neighbor information.

**show interfaces status**
- Provides a quick overview of interface status.

**show interfaces description**
- Displays interface descriptions and status.

**show interfaces counters errors**
- Shows error counters on interfaces.

**show port-security**
- Displays port security settings.

**show running-config interface [INTERFACE_NAME]**
- Shows the running configuration of a specific interface.

**show ip interface brief**
- Summarizes IP interface status and configuration.

**vlan [VLAN_ID]**
- Creates a VLAN or enters VLAN configuration mode.

**interface range [INTERFACE_RANGE]**
- Configures multiple interfaces at once.

**switchport mode access**
- Sets an interface to access mode.

**switchport mode trunk**
- Sets an interface to trunk mode.

**switchport access vlan [VLAN_ID]**
- Assigns an access VLAN to an interface.

**switchport trunk allowed vlan [VLAN_LIST]**
- Specifies VLANs allowed on a trunk interface.

**channel-group [NUMBER] mode active**
- Configures an interface to actively form an EtherChannel.

**show logging**
- Displays the system log.

**copy running-config startup-config**
- Saves the current configuration to the startup configuration.

**show version**
- Displays system hardware and software status.

**show inventory**
- Shows hardware inventory details.

**show environment**
- Displays environmental information like temperature and power supply status.

**show power inline**
- Shows Power over Ethernet (PoE) status on interfaces.

**show interface [INTERFACE_NAME] switchport**
- Displays switchport information for a specific interface.

**show interface [INTERFACE_NAME] status**
- Shows the status of a specific interface.

**show redundancy**
- Displays redundancy information (useful in MLAG setups).

**show boot**
- Shows boot path and image information.

**show flash:**
- Displays the contents of the flash memory.

**show processes cpu**
- Shows CPU utilization.

**show processes memory**
- Displays memory usage.

**show interface [INTERFACE_NAME] counters**
- Shows detailed counters for a specific interface.

**show access-lists**
- Displays configured access lists.

**show arp**
- Shows the ARP table.

**show interface [INTERFACE_NAME] capabilities**
- Displays the capabilities of a specific interface.

**show sdm prefer**
- Shows the Switch Database Management (SDM) template in use.

**show standby**
- Displays Hot Standby Router Protocol (HSRP) information.

**show udld [INTERFACE_NAME]**
- Shows UniDirectional Link Detection (UDLD) status on an interface.

**show vlan brief**
- Provides a brief summary of all VLANs.

**show vrf**
- Displays VRF (Virtual Routing and Forwarding) information.

**show mlag**
- Displays MLAG configuration and status.

**mlag peer**
- Configures MLAG peer settings.

**mlag domain**
- Configures an MLAG domain.

**show mlag detail**
- Provides detailed information about MLAG status.

**debug spanning-tree events**
- Enables debugging for STP events.

**test cable-diagnostics tdr interface [INTERFACE_NAME]**
- Runs a Time Domain Reflectometer (TDR) test on a specified interface.

**show interfaces**
- Displays status and statistics for all interfaces.

**show interfaces [interface]**
- Shows detailed information about a specific interface.

**show interfaces status**
- Provides a quick overview of all interfaces' status.

**show mac address-table**
- Displays the MAC address table.

**show mac address-table dynamic**
- Shows dynamically learned MAC addresses.

**show vlan**
- Displays VLAN information.

**show vlan brief**
- Provides a brief summary of all VLANs.

**show spanning-tree**
- Displays Spanning Tree Protocol (STP) information.

**show spanning-tree summary**
- Shows a summary of STP status.

**show spanning-tree root**
- Displays the root bridge information for STP.

**show spanning-tree [vlan VLAN_ID]**
- Shows STP information for a specific VLAN.

**show interfaces trunk**
- Displays trunk ports and their encapsulation.

**show etherchannel summary**
- Shows the status of EtherChannel interfaces.

**show port-security**
- Displays port security settings.

**show port-security interface [interface]**
- Shows port security details on a specific interface.

**show interfaces switchport**
- Displays switchport information for interfaces.

**show arp**
- Displays the ARP table.

**show cdp neighbors**
- Lists Cisco Discovery Protocol (CDP) neighbor devices.

**show cdp neighbors detail**
- Provides detailed information about CDP neighbors.

**show lldp neighbors**
- Lists Link Layer Discovery Protocol (LLDP) neighbor devices.

**show lldp neighbors detail**
- Provides detailed information about LLDP neighbors.

**test cable-diagnostics tdr interface [interface]**
- Runs a cable diagnostics test on an interface.

**show interface counters errors**
- Displays interface error counters.

**show interface [interface] switchport**
- Shows the switchport configuration for a specific interface.

**show vtp status**
- Displays VLAN Trunking Protocol (VTP) status.

**show udld [interface]**
- Shows UniDirectional Link Detection (UDLD) status on an interface.

**clear mac address-table dynamic**
- Clears the dynamic entries from the MAC address table.

**clear counters [interface]**
- Clears the counters on a specific interface.

**debug spanning-tree events**
- Enables debugging for STP events.

**show logging**
- Displays the system log for potential error messages or alerts.

**show spanning-tree [vlan VLAN_ID]**
- Shows STP information for a specific VLAN.

**show interfaces trunk**
- Displays trunk ports and their encapsulation.

**show etherchannel summary**
- Shows the status of EtherChannel interfaces.

**show port-security**
- Displays port security settings.

**show port-security interface [interface]**
- Shows port security details on a specific interface.

**show interfaces switchport**
- Displays switchport information for interfaces.

**show arp**
- Displays the ARP table.

**show cdp neighbors**
- Lists Cisco Discovery Protocol (CDP) neighbor devices.

**show cdp neighbors detail**
- Provides detailed information about CDP neighbors.

**show lldp neighbors**
- Lists Link Layer Discovery Protocol (LLDP) neighbor devices.

**show lldp neighbors detail**
- Provides detailed information about LLDP neighbors.

**test cable-diagnostics tdr interface [interface]**
- Runs a cable diagnostics test on an interface.

**show interface counters errors**
- Displays interface error counters.

**show interface [interface] switchport**
- Shows the switchport configuration for a specific interface.

**show vtp status**
- Displays VLAN Trunking Protocol (VTP) status.

**show udld [interface]**
- Shows UniDirectional Link Detection (UDLD) status on an interface.

**clear mac address-table dynamic**
- Clears the dynamic entries from the MAC address table.

**clear counters [interface]**
- Clears the counters on a specific interface.

**debug spanning-tree events**
- Enables debugging for STP events.

**show logging**
- Displays the system log for potential error messages or alerts.
