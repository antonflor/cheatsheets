# Arista Command Cheat Sheet

This cheat sheet compiles a range of commands specifically for network engineers working with Arista switches. It covers various aspects of network troubleshooting and configuration, including OSPF, Layer 2 issues, and general device management.

Intended as a quick reference guide, this document aims to streamline the process of managing Arista network environments and to assist in efficient problem-solving. Remember to apply these commands judiciously, as they can have a substantial impact on network performance and stability.


## BGP (Border Gateway Protocol) Management

- **show ip bgp**
  - Shows the BGP routing table.

- **show ip bgp summary**
  - Provides a summary of BGP neighbor relationships.

- **show ip bgp neighbors**
  - Displays detailed information about BGP neighbors.

- **router bgp [AS_NUMBER]**
  - Configures BGP with a specific Autonomous System (AS) number.

- **neighbor [IP_ADDRESS] remote-as [AS_NUMBER]**
  - Configures a BGP neighbor.

- **network [IP_ADDRESS] mask [NETMASK]**
  - Advertises a network in BGP.

- **show ip bgp neighbors [IP_ADDRESS] advertised-routes**
  - Displays the routes advertised to a specific BGP neighbor.

- **show ip bgp neighbors [IP_ADDRESS] received-routes**
  - Shows routes received from a specific BGP neighbor.

- **show ip bgp regexp [REGEXP]**
  - Displays routes matching a specific regular expression in BGP.

- **clear ip bgp [IP_ADDRESS]**
  - Clears a specific BGP session.

- **show ip bgp community [COMMUNITY]**
  - Displays routes with a specific BGP community tag.

- **show ip bgp dampening parameters**
  - Displays the BGP route dampening parameters.

- **show ip bgp filter-list [LIST_NUMBER]**
  - Shows routes that match a specific BGP filter list.

- **show ip bgp flap-statistics**
  - Displays BGP flap statistics.

- **show ip bgp paths**
  - Shows path information for BGP routes.

- **show ip bgp policy**
  - Displays the BGP policy information.

- **show ip bgp route-map [MAP_NAME]**
  - Shows routes that match a specific route-map in BGP.

- **show ip bgp summary longer-prefixes**
  - Displays a summary of BGP with longer prefixes.

- **show ip bgp update-group**
  - Shows BGP update group information.

- **show running-config bgp**
  - Displays the current BGP configuration.

## VRRP (Virtual Router Redundancy Protocol)

- **show vrrp**
  - Displays VRRP configurations and status.

- **show running-config vrrp**
  - Displays the running configuration specific to VRRP.

- **vrrp [GROUP_ID] ip [IP_ADDRESS]**
  - Configures a VRRP group with an IP address.

- **vrrp [GROUP_ID] priority [PRIORITY_LEVEL]**
  - Sets the priority for a VRRP group.

- **vrrp [GROUP_ID] preempt**
  - Enables preemption in a VRRP group.

- **show vrrp brief**
  - Provides a brief overview of VRRP status.

- **show vrrp detail**
  - Displays detailed VRRP information.

## VLAN and Interface Configuration

- **show interfaces Vlan [VLAN_ID]**
  - Shows configuration and status of a specific VLAN interface.

- **vlan [VLAN_ID]**
  - Creates a VLAN or enters VLAN configuration mode.

- **interface Vlan [VLAN_ID]**
  - Enters configuration mode for a specific VLAN interface.

- **show vlan**
  - Displays VLAN information and status.

- **show interfaces trunk**
  - Shows trunk interface status and configurations.

- **spanning-tree vlan [VLAN_ID] priority [PRIORITY]**
  - Sets the spanning-tree priority for a VLAN.

- **show spanning-tree detail**
  - Displays detailed spanning-tree information.

## OSPF (Open Shortest Path First)

- **show ip ospf neighbor**
  - Displays OSPF neighbor relationships and states.

- **show ip ospf interface**
  - Shows OSPF-related information on interfaces.

- **show ip ospf database**
  - Displays the OSPF link-state database (LSDB).

- **show ip route ospf**
  - Shows routes learned via OSPF.

- **show ip ospf traffic**
  - Displays OSPF traffic statistics.

- **show ip ospf border-routers**
  - Shows OSPF border routers in the network.

- **show ip ospf virtual-links**
  - Displays OSPF virtual link configurations.

- **debug ip ospf events**
  - Enables OSPF event debugging for more granular troubleshooting.

## General Network Management

- **configure terminal**
  - Enters global configuration mode.

- **show ip route vrf [VRF_NAME]**
  - Shows the IP routing table for a specific VRF.

- **show vrf**
  - Displays VRF configurations and status.

- **ip route [DESTINATION] [MASK] [NEXT_HOP_IP]**
  - Configures a static route.

- **show ip interface brief**
  - Summarizes the status and configuration of all IP interfaces.

- **show interfaces description**
  - Displays interface descriptions and status.

- **show interfaces status**
  - Shows the link status of all interfaces.

- **show interfaces counters errors**
  - Displays interface error counters.

- **show interfaces counters rate**
  - Shows the rate of traffic on interfaces.

- **write memory**
  - Saves the current configuration to the startup configuration file.

- **show logging**
  - Displays the system log for potential error messages or alerts.

- **show mlag detail**
  - Provides detailed information on MLAG status.

- **show mac address-table**
  - Shows the MAC address table.

- **show etherchannel summary**
  - Provides a summary of EtherChannel status.

- **show ip arp**
  - Shows the ARP table, useful for resolving IP to MAC address mappings.

- **show lldp neighbors**
  - Displays LLDP neighbor information, useful for verifying network topology.
