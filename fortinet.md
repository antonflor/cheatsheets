### Fortinet Cheat Sheet

#### Introduction to Fortinet
Fortinet provides a range of network security products, with FortiGate being their flagship next-generation firewall (NGFW) solution.

- **Purpose**: Network security, firewall management, and threat prevention.

#### Configuration Commands
**Accessing FortiGate CLI**
- Access via console, SSH, or web interface.

**Viewing Configuration**
- `show`: Displays current configuration.
- `get system status`: Shows the system status and configuration.

**Setting Hostname**
- `config system global`
- `set hostname [hostname]`
- `end`

**Configuring Interfaces**
- `config system interface`
- `edit [interface_name]`
- `set ip [IP_address] [subnet_mask]`
- `set allowaccess [access_methods]`
- `next`
- `end`

#### Network and Routing Configuration
**Static Routing**
- `config router static`
- `edit 0`
- `set device [interface_name]`
- `set gateway [gateway_IP]`
- `next`
- `end`

**DHCP Server Configuration**
- `config system dhcp server`
- `edit 0`
- `set interface [interface_name]`
- `config ip-range`
- `edit 1`
- `set start-ip [start_IP]`
- `set end-ip [end_IP]`
- `next`
- `end`

#### Firewall Policies and Security Profiles
**Creating Firewall Policies**
- `config firewall policy`
- `edit 0`
- `set srcintf [source_interface]`
- `set dstintf [destination_interface]`
- `set srcaddr [source_address]`
- `set dstaddr [destination_address]`
- `set action accept`
- `set schedule always`
- `set service [service_name]`
- `next`
- `end`

**Configuring Security Profiles**
- `config firewall profile-protocol-options`
- `edit "default"`
- `set [protocol_option]`
- `next`
- `end`

#### VPN Configuration
**IPsec VPN Setup**
- `config vpn ipsec phase1-interface`
- `edit "vpnname"`
- `set interface [interface_name]`
- `set ike-version 2`
- `set keylife 28800`
- `set peertype any`
- `set proposal aes256-sha256`
- `next`
- `end`

**High Availability (HA) Configuration**
- `config system ha`
- `set mode active-passive`
- `set group-name "HA_Group"`
- `set hbdev "[interface] [priority]"`
- `end`

**VLAN Configuration**
- `config system interface`
- `edit [vlan_name]`
- `set vdom [vdom_name]`
- `set ip [IP_address] [subnet_mask]`
- `set interface [physical_interface]`
- `set vlanid [VLAN_ID]`
- `next`
- `end`

#### Troubleshooting and Diagnostics
**Basic Diagnostics**
- `diagnose debug flow trace start 100`
- `diagnose debug enable`
- `diagnose debug disable`

**Log Viewing**
- `diagnose log test`
- `get log [log_type] [number]`

#### Tips for Using Fortinet
- **Regular Updates**: Keep FortiGate firmware and definitions updated.
- **Backup Configuration**: Regularly backup your configuration.
- **Monitoring**: Use FortiGate's logging and monitoring features for insights.
