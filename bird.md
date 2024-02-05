**birdc show status**
- Displays the current status of the BIRD routing daemon.

**birdc show protocols**
- Lists all routing protocols configured in BIRD.

**birdc show protocol [PROTOCOL_NAME]**
- Shows detailed information about a specific routing protocol.

**birdc show route**
- Displays the routing table managed by BIRD.

**birdc show route protocol [PROTOCOL_NAME]**
- Shows routes learned from a specific routing protocol.

**birdc show route all**
- Displays all routes in the routing table, including inactive ones.

**birdc show route export [PROTOCOL_NAME]**
- Shows routes exported by a specific protocol.

**birdc show route where [CONDITION]**
- Displays routes matching a specific condition.

**birdc show route for [IP_ADDRESS]**
- Shows the route for a specific IP address.

**birdc show ospf neighbors**
- Lists OSPF neighbors.

**birdc show ospf state**
- Displays the OSPF internal state.

**birdc show ospf interfaces**
- Shows OSPF interfaces and their states.

**birdc show ospf lsadb**
- Displays the OSPF link-state database.

**birdc show bfd sessions**
- Lists BFD (Bidirectional Forwarding Detection) sessions.

**birdc show memory**
- Shows memory usage by the BIRD daemon.

**birdc configure**
- Reloads the configuration file.

**birdc configure check**
- Checks the configuration file for errors without applying changes.

**birdc disable [PROTOCOL_NAME]**
- Temporarily disables a routing protocol.

**birdc enable [PROTOCOL_NAME]**
- Enables a previously disabled routing protocol.

**birdc restart [PROTOCOL_NAME]**
- Restarts a specific routing protocol.

**birdc log [MESSAGE]**
- Sends a custom log message to the BIRD log.

**birdc show events**
- Displays recent routing events.

**birdc show symbols**
- Lists all symbols (like route filters and functions) defined in the configuration.

**birdc debug [PROTOCOL_NAME] [DEBUG_OPTIONS]**
- Enables debugging for a specific protocol.

**birdc show filters**
- Displays the configured route filters.

**birdc show interfaces**
- Lists network interfaces known to BIRD.

**birdc show sockets**
- Shows active network sockets used by BIRD.

**birdc show tables**
- Lists routing tables managed by BIRD.

**birdc show static routes**
- Displays static routes configured in BIRD.

**birdc show rip neighbors**
- Lists RIP neighbors and their status.
