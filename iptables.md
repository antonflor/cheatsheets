# iptables Command Cheat Sheet

This document provides a comprehensive list of iptables commands, crucial for network administrators and security professionals. iptables is the default firewall tool on Linux systems, used for network packet filtering and manipulation.

The cheat sheet covers basic to advanced iptables functionalities, including setting up firewalls, NAT, port forwarding, and managing traffic rules. It serves as a handy guide for securing Linux systems and managing network traffic flows. Given the powerful nature of iptables, it is advised to use these commands with caution, as incorrect usage can lead to network outages or security vulnerabilities.

---

1. **List All Rules**
   - `sudo iptables -L`
   - Lists all active rules.
2. **List Rules with Numbers**
   - `sudo iptables -L --line-numbers`
   - Lists rules with line numbers for easier management.
3. **Delete Rule by Number**
   - `sudo iptables -D INPUT [LINE_NUMBER]`
   - Deletes a specific rule from the INPUT chain.
4. **Set Default Policy**
   - `sudo iptables -P [CHAIN] [POLICY]`
   - Sets the default policy (e.g., ACCEPT, DROP) for a chain (e.g., INPUT, OUTPUT).
5. **Allow Specific Port (TCP)**
   - `sudo iptables -A INPUT -p tcp --dport [PORT] -j ACCEPT`
   - Allows incoming traffic on a specific TCP port.
6. **Allow Specific Port (UDP)**
   - `sudo iptables -A INPUT -p udp --dport [PORT] -j ACCEPT`
   - Allows incoming traffic on a specific UDP port.
7. **Drop Traffic from an IP Address**
   - `sudo iptables -A INPUT -s [IP_ADDRESS] -j DROP`
   - Blocks all incoming traffic from a specific IP address.
8. **Allow Traffic from an IP Address**
   - `sudo iptables -A INPUT -s [IP_ADDRESS] -j ACCEPT`
   - Allows all incoming traffic from a specific IP address.
9. **Reject Traffic on a Port**
   - `sudo iptables -A INPUT -p tcp --dport [PORT] -j REJECT`
   - Rejects traffic on a specific port.
10. **Save iptables Rules**
    - `sudo iptables-save > /etc/iptables/rules.v4`
    - Saves the current rules to a file (Debian-based systems).
11. **Flush All Rules**
    - `sudo iptables -F`
    - Removes all rules.

---

1. **Log Dropped Packets**
   - `sudo iptables -A INPUT -j LOG --log-prefix "IPTables-Dropped: " --log-level 4`
   - Logs dropped packets for debugging.
2. **Limit Connections per Second**
   - `sudo iptables -A INPUT -p tcp --dport [PORT] -m limit --limit [RATE] -j ACCEPT`
   - Limits the number of connections per second to a port.
3. **Port Forwarding**
   - `sudo iptables -t nat -A PREROUTING -p tcp --dport [PORT] -j DNAT --to-destination [DEST_IP]:[DEST_PORT]`
   - Forwards traffic from one port to another IP and port.
4. **Masquerade (NAT)**
   - `sudo iptables -t nat -A POSTROUTING -o [OUT_INTERFACE] -j MASQUERADE`
   - Enables NAT for outgoing traffic on an interface.
5. **Drop Invalid Packets**
   - `sudo iptables -A INPUT -m state --state INVALID -j DROP`
   - Drops packets that are invalid.
6. **Allow Established and Related Connections**
   - `sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT`
   - Allows incoming traffic related to established connections.
7. **Block Outgoing Traffic to a Domain**
   - `sudo iptables -A OUTPUT -p tcp -d [DOMAIN] --dport 80 -j DROP`
   - Blocks outgoing HTTP traffic to a specific domain.
8. **Redirect Traffic to Another Port**
   - `sudo iptables -t nat -A PREROUTING -p tcp --dport [PORT] -j REDIRECT --to-port [NEW_PORT]`
   - Redirects traffic from one port to another port on the same machine.
9. **Block Ping Requests**
   - `sudo iptables -A INPUT -p icmp --icmp-type echo-request -j DROP`
   - Blocks ICMP echo requests (ping).
10. **Allow Traffic on Multiple Ports**
    - `sudo iptables -A INPUT -p tcp -m multiport --dports [PORT1],[PORT2],[PORT3] -j ACCEPT`
    - Allows traffic on multiple specified ports.
11. **Rate Limiting Incoming Connections**
    - `sudo iptables -A INPUT -p tcp --dport [PORT] -m state --state NEW -m recent --set`
    - `sudo iptables -A INPUT -p tcp --dport [PORT] -m state --state NEW -m recent --update --seconds [SECONDS] --hitcount [HITCOUNT] -j DROP`
    - Limits new connections to a port within a given timeframe.
12. **Block Traffic from a Specific Network**
    - `sudo iptables -A INPUT -s [NETWORK/MASK] -j DROP`
    - Blocks all incoming traffic from a specific network.
13. **Allow Traffic Only from a Specific Network**
    - `sudo iptables -A INPUT -s [NETWORK/MASK] -j ACCEPT`
    - Allows traffic only from a specific network.
14. **Log New Connections**
    - `sudo iptables -A INPUT -m state --state NEW -j LOG --log-prefix "New Connection: "`
    - Logs new incoming connections.
15. **Drop Outgoing Traffic to a Specific Port**
    - `sudo iptables -A OUTPUT -p tcp --dport [PORT] -j DROP`
    - Blocks outgoing traffic to a specific port.
16. **Redirect All HTTP Traffic to HTTPS**
    - `sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 443`
    - Redirects all HTTP traffic to HTTPS.
17. **Block Outgoing SMTP Mail**
    - `sudo iptables -A OUTPUT -p tcp --dport 25 -j REJECT`
    - Blocks outgoing SMTP mail.
18. **Allow SSH Access from a Specific Network**
    - `sudo iptables -A INPUT -p tcp --dport 22 -s [NETWORK/MASK] -j ACCEPT`
    - Allows SSH access only from a specific network.
19. **Block Outgoing Telnet**
    - `sudo iptables -A OUTPUT -p tcp --dport 23 -j DROP`
    - Blocks outgoing Telnet connections.
