# BIRD Command Cheat Sheet

This document provides a comprehensive list of commands for managing and troubleshooting BIRD (BIRD Internet Routing Daemon), an open-source routing daemon for Unix-like systems. BIRD is widely used for its flexibility and supports various routing protocols, making it a preferred choice in many network infrastructures.

The commands listed here are tailored for network engineers and system administrators who work with BIRD on Debian-based systems. They cover a range of functionalities from basic status checks to advanced protocol management and routing table manipulation. This cheat sheet serves as a quick reference guide to streamline daily tasks and enhance troubleshooting procedures.

Please note that the effectiveness and output of these commands can vary based on your BIRD configuration and the specific environment in which it's running. Always ensure you have the necessary permissions to execute these commands and use them cautiously, especially in a production environment.


## BIRD Status and Protocol Management

- **birdc show status**
  - Displays the current status of the BIRD routing daemon.

- **birdc show protocols**
  - Lists all routing protocols configured in BIRD.

- **birdc show protocol [PROTOCOL_NAME]**
  - Shows detailed information about a specific routing protocol.

- **birdc configure**
  - Reloads the configuration file.

- **birdc configure check**
  - Checks the configuration file for errors without applying changes.

- **birdc disable [PROTOCOL_NAME]**
  - Temporarily disables a routing protocol.

- **birdc enable [PROTOCOL_NAME]**
  - Enables a previously disabled routing protocol.

- **birdc restart [PROTOCOL_NAME]**
  - Restarts a specific routing protocol.

- **birdc log [MESSAGE]**
  - Sends a custom log message to the BIRD log.

- **birdc show events**
  - Displays recent routing events.

- **birdc show symbols**
  - Lists all symbols (like route filters and functions) defined in the configuration.

- **birdc debug [PROTOCOL_NAME] [DEBUG_OPTIONS]**
  - Enables debugging for a specific protocol.

## Route Display and Analysis

- **birdc show route**
  - Displays the routing table managed by BIRD.

- **birdc show route protocol [PROTOCOL_NAME]**
  - Shows routes learned from a specific routing protocol.

- **birdc show route all**
  - Displays all routes in the routing table, including inactive ones.

- **birdc show route export [PROTOCOL_NAME]**
  - Shows routes exported by a specific protocol.

- **birdc show route where [CONDITION]**
  - Displays routes matching a specific condition.

- **birdc show route for [IP_ADDRESS]**
  - Shows the route for a specific IP address.

## OSPF and BFD Sessions

- **birdc show ospf neighbors**
  - Lists OSPF neighbors.

- **birdc show ospf state**
  - Displays the OSPF internal state.

- **birdc show ospf interfaces**
  - Shows OSPF interfaces and their states.

- **birdc show ospf lsadb**
  - Displays the OSPF link-state database.

- **birdc show bfd sessions**
  - Lists BFD (Bidirectional Forwarding Detection) sessions.

## Memory Usage and Interface Information

- **birdc show memory**
  - Shows memory usage by the BIRD daemon.

- **birdc show interfaces**
  - Lists network interfaces known to BIRD.

- **birdc show sockets**
  - Shows active network sockets used by BIRD.

## Routing Tables and Filters

- **birdc show tables**
  - Lists routing tables managed by BIRD.

- **birdc show static routes**
  - Displays static routes configured in BIRD.

- **birdc show filters**
  - Displays the configured route filters.

- **birdc show rip neighbors**
  - Lists RIP neighbors and their status.
