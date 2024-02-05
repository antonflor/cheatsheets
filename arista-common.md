# Arista Command Cheat Sheet

This cheat sheet compiles a range of commands specifically for network engineers working with Arista switches. It covers various aspects of network troubleshooting and configuration, including OSPF, Layer 2 issues, and general device management.

Intended as a quick reference guide, this document aims to streamline the process of managing Arista network environments and to assist in efficient problem-solving. Remember to apply these commands judiciously, as they can have a substantial impact on network performance and stability.

---

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

**show ip ospf neighbor**
- Displays OSPF neighbor relationships and states.

**show ip ospf interface**
- Shows OSPF-related information on interfaces.

**show ip ospf database**
- Displays the OSPF link-state database (LSDB).

**show ip route ospf**
- Shows routes learned via OSPF.

**show ip ospf traffic**
- Displays OSPF traffic statistics.

**show spanning-tree**
- Shows the state of Spanning Tree Protocol (STP) on the switch.

**show spanning-tree detail**
- Provides detailed information about STP instances.

**show interfaces status**
- Displays the status of all interfaces.

**show interfaces description**
- Shows a description of interfaces and their status.

**show vlan**
- Displays VLAN configurations and status.

**show mac address-table**
- Shows the MAC address table.

**show etherchannel summary**
- Provides a summary of EtherChannel status.

**show interfaces trunk**
- Displays trunking information on interfaces.

**show interfaces [INTERFACE_NAME]**
- Displays detailed information about a specific interface.

**show logging**
- Displays the system log for potential error messages or alerts.

**show ip arp**
- Shows the ARP table, useful for resolving IP to MAC address mappings.

**show lldp neighbors**
- Displays LLDP neighbor information, useful for verifying network topology.

**show ip ospf border-routers**
- Shows OSPF border routers in the network.

**show ip ospf virtual-links**
- Displays OSPF virtual link configurations.

**debug ip ospf events**
- Enables OSPF event debugging for more granular troubleshooting.

**show ip bgp summary**
- Displays a summary of BGP peerings and their status.

**show ip bgp neighbors**
- Shows detailed information about BGP neighbors.

**show ip bgp**
- Displays the BGP routing table.

**show ip bgp neighbors [NEIGHBOR_IP] received-routes**
- Shows routes received from a specific BGP neighbor.

**show ip bgp neighbors [NEIGHBOR_IP] advertised-routes**
- Displays the routes advertised to a specific BGP neighbor.

**show ip route**
- Shows the IP routing table.

**show ip route summary**
- Provides a summary of the IP routing table.

**show ip bgp regexp [REGEXP]**
- Displays routes matching a specific regular expression in BGP.

**show ip bgp community [COMMUNITY]**
- Shows routes with a specific BGP community tag.

**clear ip bgp [NEIGHBOR_IP]**
- Clears a specific BGP session.

**show ip bgp dampening parameters**
- Displays the BGP route dampening parameters.

**show ip bgp paths**
- Shows path information for BGP routes.

**show ip bgp neighbors [NEIGHBOR_IP] routes**
- Displays the routes learned from a specific BGP neighbor.

**show ip bgp flap-statistics**
- Displays BGP flap statistics.

**show ip bgp neighbors [NEIGHBOR_IP] advertised-routes detail**
- Shows detailed information about routes advertised to a specific neighbor.

**show ip bgp neighbors [NEIGHBOR_IP] received-routes detail**
- Displays detailed information about routes received from a specific neighbor.

**show ip bgp inconsistent-as**
- Shows BGP paths with inconsistent origin AS numbers.

**show ip bgp summary | no-more**
- Displays the BGP summary without pagination.

**show ip bgp filter-list [FILTER_LIST]**
- Shows routes that match a specific BGP filter list.

**debug ip bgp updates**
- Enables debugging for BGP updates.
