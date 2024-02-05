# Juniper Command Cheat Sheet

This document presents a comprehensive list of Juniper commands, focusing on BGP, MPLS, and general routing management. It is an invaluable resource for network engineers and administrators involved in managing Juniper routers and switches.

The commands provided here are intended to assist in everyday network operations and advanced troubleshooting. They offer insights into protocol configurations, routing tables, and network diagnostics. Use these commands carefully, as they can affect network behavior and service delivery.


---

**show bgp summary**
  - Displays a summary of BGP peerings and their status.

**show bgp neighbor [NEIGHBOR_IP]**
  - Shows detailed information about a specific BGP neighbor.

**show route protocol bgp**
  - Displays BGP routes in the routing table.

**show route receive-protocol bgp [NEIGHBOR_IP]**
  - Shows routes received from a specific BGP neighbor.

**show route advertising-protocol bgp [NEIGHBOR_IP]**
  - Displays routes being advertised to a specific BGP neighbor.

**show bgp group [GROUP_NAME]**
  - Displays configuration and statistics for a specific BGP group.

**clear bgp neighbor [NEIGHBOR_IP]**
  - Clears a specific BGP session.

**show mpls lsp**
  - Shows MPLS Label Switched Path (LSP) information.

**show mpls interface**
  - Displays MPLS interface information.

**show route table mpls.0**
  - Displays the MPLS routing table.

**show mpls lsp extensive**
  - Shows detailed information about MPLS LSPs.

**show mpls lsp statistics**
  - Displays statistics for MPLS LSPs.

**traceoptions flag mpls**
  - Enables MPLS-specific tracing options for debugging.

**show route table inet.3**
  - Displays the MPLS forwarding table.

**show mpls lsp name [LSP_NAME] detail**
  - Shows detailed information about a specific MPLS LSP.

**show bgp neighbor [NEIGHBOR_IP] detail**
  - Displays detailed information about BGP session and routes for a specific neighbor.

**show route advertising-protocol bgp [NEIGHBOR_IP] detail**
  - Shows detailed routes being advertised to a specific BGP neighbor.

**show route receive-protocol bgp [NEIGHBOR_IP] detail**
  - Displays detailed routes received from a specific BGP neighbor.

**show bgp community [COMMUNITY]**
  - Shows routes with a specific BGP community.

**monitor traffic interface [INTERFACE_NAME] no-resolve**
  - Monitors real-time traffic on a specified interface, useful for MPLS troubleshooting.

**show bgp summary**
   - Displays a summary of BGP peerings and their status.

**show route protocol bgp**
   - Shows BGP routes in the routing table.

**show bgp neighbor [NEIGHBOR_IP]**
   - Displays detailed information about a specific BGP neighbor.

**set protocols bgp group [GROUP_NAME] type [TYPE]**
   - Configures a BGP group with a specific type.

**set protocols bgp group [GROUP_NAME] neighbor [NEIGHBOR_IP]**
   - Adds a neighbor to a BGP group.

**show mpls interface**
   - Displays MPLS interface information.

**show mpls lsp**
   - Shows MPLS Label Switched Path (LSP) information.

**set protocols mpls label-switched-path [LSP_NAME] to [DESTINATION]**
   - Configures an MPLS LSP.

**show route table mpls.0**
   - Displays the MPLS routing table.

**show interfaces irb**
- Shows Integrated Routing and Bridging (IRB) interface information.

**show interfaces irb detail**
- Displays detailed information about IRB interfaces.

**set interfaces irb unit [UNIT_NUMBER] family inet address [IP_ADDRESS]**
- Configures an IP address on an IRB interface.

**commit**
- Commits the current candidate configuration.

**commit check**
- Checks the candidate configuration for errors without committing.

**commit confirmed [TIME]**
- Commits the configuration with an automatic rollback after a specified time unless confirmed.

**show ethernet-switching table**
- Displays the Ethernet switching table (MAC address table).

**show ethernet-switching table | no-more**
- Shows the Ethernet switching table without pagination.

**set routing-options static route [DESTINATION] next-hop [NEXT_HOP]**
- Configures a static route.

**show route advertising-protocol bgp [NEIGHBOR_IP]**
- Displays routes being advertised to a specific BGP neighbor.

**show route receive-protocol bgp [NEIGHBOR_IP]**
- Shows routes received from a specific BGP neighbor.

**show bgp group**
- Displays BGP group configurations.

**show configuration protocols bgp**
- Shows the BGP configuration.

**show configuration | display set | match bgp**
- Displays BGP configuration in 'set' command format.

**show route table inet.0**
- Displays the IPv4 unicast routing table.

**show route table inet6.0**
- Displays the IPv6 unicast routing table.

**show route table [TABLE_NAME] extensive**
- Shows detailed information about a specific routing table.

**show route forwarding-table**
- Displays the forwarding table.

**show route best**
- Shows the best routes in the routing table.

**show route aspath-regex [AS_PATH_REGEX]**
- Displays routes matching a specific AS path regular expression.

**show route community [COMMUNITY]**
- Shows routes with a specific BGP community.

**show route extensive**
- Displays extensive route information.

**show route filter [FILTER_NAME]**
- Shows routes matching a specific route filter.

**show route inactive**
- Displays inactive routes in the routing table.

**show route next-hop [NEXT_HOP_IP]**
- Shows routes with a specific next-hop.

**show route protocol [PROTOCOL]**
- Displays routes learned from a specific protocol.

**show route summary**
- Provides a summary of the routing table.

**show route terse**
- Shows a terse output of the routing table.

**show route advertising-protocol bgp [NEIGHBOR_IP] extensive**
- Shows detailed information about routes advertised to a BGP neighbor.

**show route receive-protocol bgp [NEIGHBOR_IP] extensive**
- Shows detailed information about routes received from a BGP neighbor.

**show bgp neighbor [NEIGHBOR_IP] advertised-routes**
- Displays routes advertised to a specific BGP neighbor.

**show bgp neighbor [NEIGHBOR_IP] received-routes**
- Shows routes received from a specific BGP neighbor.

**show bgp neighbor [NEIGHBOR_IP] routes**
- Displays routes learned from a specific BGP neighbor.

**show bgp summary | no-more**
- Shows a summary of BGP peerings without pagination.

**show mpls lsp name [LSP_NAME]**
- Displays information about a specific MPLS LSP.

**show mpls lsp name [LSP_NAME] detail**
- Shows detailed information about a specific MPLS LSP.

**show mpls lsp statistics**
- Displays MPLS LSP statistics.

**show mpls lsp extensive**
- Shows extensive information about MPLS LSPs.

**show mpls lsp summary**
- Provides a summary of MPLS LSPs.

**show mpls lsp | no-more**
- Displays MPLS LSP information without pagination.

**show mpls lsp | match [PATTERN]**
- Shows MPLS LSP information matching a specific pattern.
