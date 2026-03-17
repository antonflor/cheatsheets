# Fortinet / FortiGate Cheat Sheet

## Introduction to Fortinet

Fortinet provides a range of network security products, with FortiGate being their flagship next-generation firewall (NGFW) solution. FortiGate integrates firewalling, VPN, intrusion prevention, web filtering, antivirus, and SD-WAN into a single platform.

- **Purpose**: Network security, firewall management, threat prevention, and SD-WAN.

---

## Accessing the FortiGate CLI

```bash
# Via console cable (baud rate 9600)
# Via SSH
ssh admin@[FORTIGATE_IP]

# Enter config mode for any subsystem
config [section]

# Show current configuration
show

# Show running config (with defaults)
show full-configuration

# Get system status
get system status

# Exit config mode
end   # commit and exit
abort # discard and exit
next  # move to next entry in a list
```

---

## System and Status

```bash
# Show system status (firmware, hostname, serial)
get system status

# Show hardware info
get system performance status

# Show system resource usage (CPU, memory)
diagnose sys top

# Show system uptime
get system performance status | grep Uptime

# Show FortiGate HA status
get system ha status

# Show system time
get system clock

# Set hostname
config system global
  set hostname [HOSTNAME]
end

# Set system time
config system ntp
  set ntpsync enable
  set syncinterval 60
  set server-mode enable
  config ntpserver
    edit 1
      set server pool.ntp.org
    next
  end
end
```

---

## Interface Configuration

```bash
# View all interfaces
get system interface

# View a specific interface
get system interface [INTERFACE_NAME]

# Configure an interface
config system interface
  edit [INTERFACE_NAME]
    set ip [IP_ADDRESS] [SUBNET_MASK]
    set allowaccess ping https ssh
    set alias "WAN Link"
    set description "Primary WAN"
    set status up
  next
end

# Configure a VLAN interface
config system interface
  edit [VLAN_NAME]
    set vdom root
    set ip [IP_ADDRESS] [SUBNET_MASK]
    set interface [PHYSICAL_INTERFACE]
    set vlanid [VLAN_ID]
    set allowaccess ping
  next
end

# View interface statistics
diagnose netlink interface list
fnsysctl ifconfig [INTERFACE_NAME]
```

---

## Routing

```bash
# Show routing table
get router info routing-table all

# Show routing table for a specific protocol
get router info routing-table ospf
get router info routing-table bgp
get router info routing-table connected
get router info routing-table static

# Show BGP summary
get router info bgp summary

# Show BGP neighbors
get router info bgp neighbors

# Show OSPF neighbors
get router info ospf neighbor

# Configure a static route
config router static
  edit 0
    set device [INTERFACE_NAME]
    set gateway [GATEWAY_IP]
    set dst [DESTINATION_NETWORK] [SUBNET_MASK]
    set distance [ADMINISTRATIVE_DISTANCE]
    set priority [PRIORITY]
  next
end

# Configure a default route
config router static
  edit 0
    set device [WAN_INTERFACE]
    set gateway [ISP_GATEWAY]
  next
end

# Configure OSPF
config router ospf
  set router-id [ROUTER_ID]
  config area
    edit 0.0.0.0
    next
  end
  config network
    edit 1
      set prefix [NETWORK] [MASK]
      set area 0.0.0.0
    next
  end
end

# Configure BGP
config router bgp
  set as [AS_NUMBER]
  set router-id [ROUTER_ID]
  config neighbor
    edit [NEIGHBOR_IP]
      set remote-as [NEIGHBOR_AS]
      set activate enable
    next
  end
end
```

---

## Firewall Policies

```bash
# Show all firewall policies
show firewall policy

# Show policy stats (hit counts)
diagnose firewall iprope show [POLICY_ID]

# Create a firewall policy
config firewall policy
  edit 0
    set name "Allow-Internal-to-WAN"
    set srcintf [SOURCE_INTERFACE]
    set dstintf [DESTINATION_INTERFACE]
    set srcaddr [SOURCE_ADDRESS_OBJECT]
    set dstaddr all
    set action accept
    set schedule always
    set service ALL
    set nat enable
    set logtraffic all
  next
end

# Delete a firewall policy
config firewall policy
  delete [POLICY_ID]
end

# Show address objects
show firewall address
show firewall addrgrp

# Create an address object
config firewall address
  edit "My-Server"
    set type ipmask
    set subnet [IP_ADDRESS] [SUBNET_MASK]
  next
end

# Create an address group
config firewall addrgrp
  edit "My-Servers"
    set member "Server1" "Server2"
  next
end
```

---

## NAT Configuration

```bash
# Enable NAT on a policy (outbound PAT/masquerade)
config firewall policy
  edit [POLICY_ID]
    set nat enable
  next
end

# Create a VIP (Virtual IP / DNAT / port forwarding)
config firewall vip
  edit "WebServer-VIP"
    set extip [PUBLIC_IP]
    set extintf [WAN_INTERFACE]
    set mappedip [PRIVATE_IP]
    set portforward enable
    set extport 80
    set mappedport 80
    set protocol tcp
  next
end

# View VIPs
show firewall vip

# Create an IP pool (for outbound NAT with specific source IP)
config firewall ippool
  edit "My-IP-Pool"
    set startip [START_IP]
    set endip [END_IP]
    set type overload
  next
end
```

---

## VPN Configuration

### IPsec VPN

```bash
# Phase 1 (IKE)
config vpn ipsec phase1-interface
  edit "VPN-to-Remote"
    set interface [WAN_INTERFACE]
    set ike-version 2
    set peertype any
    set proposal aes256-sha256
    set dhgrp 14
    set remote-gw [PEER_IP]
    set psksecret [PRE_SHARED_KEY]
  next
end

# Phase 2 (IPsec)
config vpn ipsec phase2-interface
  edit "VPN-to-Remote-P2"
    set phase1name "VPN-to-Remote"
    set proposal aes256-sha256
    set src-subnet [LOCAL_SUBNET] [MASK]
    set dst-subnet [REMOTE_SUBNET] [MASK]
  next
end

# Check IPsec VPN status
diagnose vpn ike gateway list
diagnose vpn tunnel list

# Bring up/down a VPN tunnel
diagnose vpn ike gateway flush name [PHASE1_NAME]
```

### SSL VPN

```bash
# Configure SSL VPN portal
config vpn ssl web portal
  edit "full-access"
    set tunnel-mode enable
    set web-mode enable
    set ip-pools "SSLVPN_TUNNEL_ADDR1"
  next
end

# Configure SSL VPN settings
config vpn ssl settings
  set servercert [CERTIFICATE]
  set tunnel-ip-pools "SSLVPN_TUNNEL_ADDR1"
  set port 443
  set source-interface [WAN_INTERFACE]
end

# Show SSL VPN sessions
get vpn ssl monitor
diagnose vpn ssl list
```

---

## High Availability (HA)

```bash
# Configure HA
config system ha
  set mode active-passive
  set group-name "HA-Cluster"
  set group-id 1
  set password [HA_PASSWORD]
  set hbdev [HEARTBEAT_INTERFACE] 50
  set session-sync-dev [SYNC_INTERFACE]
  set priority 200
end

# Show HA status
get system ha status
diagnose sys ha status

# Force failover (on primary)
diagnose sys ha reset-uptime
```

---

## DHCP Server

```bash
# Configure DHCP server on an interface
config system dhcp server
  edit 0
    set interface [INTERFACE_NAME]
    set default-gateway [GATEWAY_IP]
    set netmask [SUBNET_MASK]
    set dns-server1 8.8.8.8
    set dns-server2 8.8.4.4
    config ip-range
      edit 1
        set start-ip [START_IP]
        set end-ip [END_IP]
      next
    end
    set lease-time 86400
  next
end

# View DHCP leases
diagnose sys dhcp server list
```

---

## Diagnostics and Troubleshooting

```bash
# Real-time packet flow trace (debug flow)
diagnose debug flow filter addr [IP_ADDRESS]
diagnose debug flow filter port [PORT]
diagnose debug flow trace start 100
diagnose debug enable
# ... reproduce the issue ...
diagnose debug flow trace stop
diagnose debug disable

# Packet capture (sniffer)
diagnose sniffer packet [INTERFACE] 'host [IP]' 4 100 l
# Syntax: diagnose sniffer packet [interface] '[filter]' [verbosity 1-6] [count] [timestamp]

# Ping from FortiGate
execute ping [IP_ADDRESS]
execute ping-options source [SOURCE_IP]
execute ping [DESTINATION_IP]

# Traceroute
execute traceroute [IP_ADDRESS]

# DNS lookup
execute nslookup name [HOSTNAME]

# Check ARP table
get system arp

# Check session table
diagnose sys session list
diagnose sys session filter dst [IP_ADDRESS]
diagnose sys session filter src [IP_ADDRESS]

# Check CPU and memory
diagnose hardware sysinfo cpu
diagnose hardware sysinfo memory

# View log entries
execute log filter category traffic
execute log display

# Check IPS/AV signature versions
diagnose autoupdate versions

# Test connectivity to FortiGuard
execute ping update.fortiguard.net
```

---

## SD-WAN

```bash
# Show SD-WAN status
get system sdwan

# Show SD-WAN health checks
diagnose sys sdwan health-check

# Show SD-WAN interface member status
diagnose sys sdwan member

# Show SD-WAN route table
diagnose sys sdwan route

# Configure SD-WAN
config system sdwan
  set status enable
  config members
    edit 1
      set interface [WAN1_INTERFACE]
      set gateway [WAN1_GATEWAY]
    next
    edit 2
      set interface [WAN2_INTERFACE]
      set gateway [WAN2_GATEWAY]
    next
  end
  config health-check
    edit "Google-DNS"
      set server 8.8.8.8
      set members 1 2
    next
  end
end
```

---

## Security Profiles

```bash
# Show antivirus profiles
show antivirus profile

# Show web filter profiles
show webfilter profile

# Show IPS sensor configurations
show ips sensor

# Show application control profiles
show application list

# Apply security profiles to a policy
config firewall policy
  edit [POLICY_ID]
    set utm-status enable
    set av-profile [AV_PROFILE_NAME]
    set ips-sensor [IPS_SENSOR_NAME]
    set webfilter-profile [WF_PROFILE_NAME]
    set application-list [APP_LIST_NAME]
    set ssl-ssh-profile [SSL_PROFILE_NAME]
  next
end
```

---

## Backup and Restore

```bash
# Backup config via CLI (TFTP)
execute backup config tftp [FILENAME] [TFTP_SERVER_IP]

# Backup config via FTP
execute backup config ftp [FILENAME] [FTP_SERVER_IP] [USERNAME] [PASSWORD]

# Restore config via TFTP
execute restore config tftp [FILENAME] [TFTP_SERVER_IP]

# Reset to factory defaults
execute factoryreset
```

---

## Tips for Using Fortinet

- **Regular Firmware Updates**: Keep FortiGate firmware updated — Fortinet regularly releases security patches and new features.
- **Backup Configuration**: Back up configuration before any change, especially firmware upgrades.
- **Use FortiManager**: For managing multiple FortiGate devices, FortiManager provides centralized policy management and deployment.
- **FortiAnalyzer / FortiSIEM**: Use FortiAnalyzer for centralized log management and reporting across your Fortinet devices.
- **Debug Flow**: `diagnose debug flow` is the most powerful tool for tracing why traffic is being allowed or dropped — learn to use it well.
- **Commit Before Changes**: In VDOM environments, always ensure you're working in the correct VDOM (`config vdom` → `edit [VDOM_NAME]`).
- **Session Table**: Use `diagnose sys session` to verify traffic is being processed and natted correctly.
