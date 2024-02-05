**show vrrp**

   - Displays VRRP (Virtual Router Redundancy Protocol) configurations and status.

**show ip bgp**

   - Shows the BGP (Border Gateway Protocol) routing table.

**show ip bgp summary**

   - Provides a summary of BGP neighbor relationships.

**show ip bgp neighbors**

   - Displays detailed information about BGP neighbors.

**show ip route vrf [VRF_NAME]**

   - Shows the IP routing table for a specific VRF (Virtual Routing and Forwarding).

**show running-config vrrp**

   - Displays the running configuration specific to VRRP.

**show interfaces Vlan [VLAN_ID]**

   - Shows configuration and status of a specific VLAN interface.

**configure terminal**

   - Enters global configuration mode.

**router bgp [AS_NUMBER]**

   - Configures BGP with a specific Autonomous System (AS) number.

**neighbor [IP_ADDRESS] remote-as [AS_NUMBER]**

- Configures a BGP neighbor.

**network [IP_ADDRESS] mask [NETMASK]**

- Advertises a network in BGP.

**show ip bgp neighbors [IP_ADDRESS] advertised-routes**

- Displays the routes advertised to a specific BGP neighbor.

**show ip bgp neighbors [IP_ADDRESS] received-routes**

- Shows routes received from a specific BGP neighbor.

**vrrp [GROUP_ID] ip [IP_ADDRESS]**

- Configures a VRRP group with an IP address.

**vrrp [GROUP_ID] priority [PRIORITY_LEVEL]**

- Sets the priority for a VRRP group.

**vrrp [GROUP_ID] preempt**

- Enables preemption in a VRRP group.

**show ip bgp regexp [REGEXP]**

- Displays routes matching a specific regular expression in BGP.

**clear ip bgp [IP_ADDRESS]**

- Clears a specific BGP session.

**show ip bgp community [COMMUNITY]**

- Displays routes with a specific BGP community tag.

**redistribute connected**

- Redistributes connected routes into BGP.

**redistribute static**

- Redistributes static routes into BGP.

**maximum-paths [NUMBER]**

- Configures BGP to use multiple paths for load sharing.

**soft-reconfiguration inbound**

- Enables soft reconfiguration for inbound BGP updates.

**show ip bgp dampening parameters**

- Displays the BGP route dampening parameters.

**show ip bgp filter-list [LIST_NUMBER]**

- Shows routes that match a specific BGP filter list.

**show ip bgp flap-statistics**

- Displays BGP flap statistics.

**show ip bgp paths**

- Shows path information for BGP routes.

**show ip bgp policy**

- Displays the BGP policy information.

**show ip bgp route-map [MAP_NAME]**

- Shows routes that match a specific route-map in BGP.

**show ip bgp summary longer-prefixes**

- Displays a summary of BGP with longer prefixes.

**show ip bgp update-group**

- Shows BGP update group information.

**show logging**

- Displays the system log files.

**show mlag detail**

- Provides detailed information on MLAG status.

**show running-config bgp**

- Displays the current BGP configuration.

**show vrf**

- Displays VRF configurations and status.

**show vrrp brief**

- Provides a brief overview of VRRP status.

**show vrrp detail**

- Displays detailed VRRP information.

**spanning-tree vlan [VLAN_ID] priority [PRIORITY]**

- Sets the spanning-tree priority for a VLAN.

**show spanning-tree detail**

- Displays detailed spanning-tree information.

**show interfaces trunk**

- Shows trunk interface status and configurations.

**show vlan**

- Displays VLAN information and status.

**vlan [VLAN_ID]**

- Creates a VLAN or enters VLAN configuration mode.

**interface Vlan [VLAN_ID]**

- Enters configuration mode for a specific VLAN interface.

**ip route [DESTINATION] [MASK] [NEXT_HOP_IP]**

- Configures a static route.

**show ip interface brief**

- Summarizes the status and configuration of all IP interfaces.

**show interfaces description**

- Displays interface descriptions and status.

**show interfaces status**

- Shows the link status of all interfaces.

**show interfaces counters errors**

- Displays interface error counters.

**show interfaces counters rate**

- Shows the rate of traffic on interfaces.

**write memory**

- Saves the current configuration to the startup configuration file.

These commands cover a broad spectrum of functionalities, including detailed BGP and VRRP configurations, interface and VLAN management, routing, and diagnostics, providing a comprehensive toolkit for managing complex network environments with Arista devices.





![User](https://lh3.googleusercontent.com/a/ALm5wu2cGgb8cVC8IXKmd1degWhGJJLa-XQaJjD3ZatDcdQ=s96-c)

You

lets do the same thing but for these would be mainly layer 2 devices and some in an mlag what are 50 commands a network engineer use to support these include stuff commands like show mac address etc etc

ChatGPT

ChatGPT

For a network engineer managing Cisco layer 2 devices, often in an MLAG (Multi-Chassis Link Aggregation) setup, here are 50 advanced and practical commands:

**show mac address-table**

   - Displays the MAC address table.
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
