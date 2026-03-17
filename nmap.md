# Nmap Cheat Sheet

This document presents a comprehensive guide to Nmap commands, tailored for security professionals, network administrators, and penetration testers. It emphasizes the utilization of Nmap for network discovery, security auditing, and vulnerability scanning. These commands are crucial for identifying open ports, detecting services and versions, operating system discovery, and performing various advanced network scans.

The cheat sheet serves as a rapid reference tool, enabling efficient network security assessments and aiding in the swift identification of potential vulnerabilities and network configurations. It is advised to use these commands judiciously, as they can profoundly influence network and host assessments, particularly in sensitive or production environments.

## Table of Contents
1. [Target Specification](#target-specification)
2. [Nmap Scan Techniques](#nmap-scan-techniques)
3. [Host Discovery](#host-discovery)
4. [Port Specification](#port-specification)
5. [Service and Version Detection](#service-and-version-detection)
6. [OS Detection](#os-detection)
7. [Timing and Performance](#timing-and-performance)
8. [NSE Scripts](#nse-scripts)
9. [Firewall / IDS Evasion and Spoofing](#firewall--ids-evasion-and-spoofing)
10. [Output](#output)
11. [Miscellaneous Nmap Flags](#miscellaneous-nmap-flags)
12. [Other Useful Nmap Commands](#other-useful-nmap-commands)

## Target Specification
- `nmap 192.168.1.1` — Scan a single IP
- `nmap 192.168.1.1 192.168.2.1` — Scan specific IPs
- `nmap 192.168.1.1-254` — Scan a range
- `nmap scanme.nmap.org` — Scan a domain
- `nmap 192.168.1.0/24` — Scan using CIDR notation
- `nmap -iL targets.txt` — Scan targets from a file
- `nmap -iR 100` — Scan 100 random hosts
- `nmap --exclude 192.168.1.1` — Exclude listed hosts
- `nmap --excludefile exclude.txt` — Exclude hosts listed in a file

## Nmap Scan Techniques
- `nmap 192.168.1.1 -sS` — TCP SYN port scan (default, requires root)
- `nmap 192.168.1.1 -sT` — TCP connect port scan (default without root privilege)
- `nmap 192.168.1.1 -sU` — UDP port scan
- `nmap 192.168.1.1 -sA` — TCP ACK port scan (firewall rule detection)
- `nmap 192.168.1.1 -sW` — TCP Window port scan
- `nmap 192.168.1.1 -sM` — TCP Maimon port scan
- `nmap 192.168.1.1 -sN` — TCP Null scan (no flags set)
- `nmap 192.168.1.1 -sF` — TCP FIN scan
- `nmap 192.168.1.1 -sX` — TCP Xmas scan (FIN, PSH, URG flags)

## Host Discovery
- `nmap 192.168.1.1-3 -sL` — No scan; list targets only
- `nmap 192.168.1.1/24 -sn` — Disable port scanning; host discovery only (ping scan)
- `nmap 192.168.1.1-5 -Pn` — Disable host discovery; port scan only (treat all hosts as online)
- `nmap 192.168.1.1 -PS22,80,443` — TCP SYN discovery on specific ports
- `nmap 192.168.1.1 -PA80` — TCP ACK discovery
- `nmap 192.168.1.1 -PU53` — UDP discovery
- `nmap 192.168.1.1 -PE` — ICMP echo request discovery
- `nmap 192.168.1.1 -PR` — ARP discovery (local network only)
- `nmap 192.168.1.1 -n` — Never do DNS resolution
- `nmap 192.168.1.1 -R` — Always resolve DNS

## Port Specification
- `nmap 192.168.1.1 -p 21` — Port scan for a single port
- `nmap 192.168.1.1 -p 21-100` — Port range
- `nmap 192.168.1.1 -p U:53,T:21-25,80` — Scan multiple TCP and UDP ports
- `nmap 192.168.1.1 -p-` — Scan all 65535 ports
- `nmap 192.168.1.1 -p http,https` — Port scan from service name
- `nmap 192.168.1.1 -F` — Fast scan (top 100 most common ports)
- `nmap 192.168.1.1 --top-ports 1000` — Scan the top 1000 most common ports

## Service and Version Detection
- `nmap 192.168.1.1 -sV` — Attempts to determine the version of the service running on port
- `nmap 192.168.1.1 -sV --version-intensity 8` — Intensity level 0–9; higher = more accurate but slower
- `nmap 192.168.1.1 -sV --version-light` — Light mode; lower accuracy, faster
- `nmap 192.168.1.1 -sV --version-all` — Intensity level 9; highest accuracy, slowest
- `nmap 192.168.1.1 -A` — Enables OS detection, version detection, script scanning, and traceroute

## OS Detection
- `nmap 192.168.1.1 -O` — Remote OS detection using TCP/IP stack fingerprinting
- `nmap 192.168.1.1 -O --osscan-limit` — Only attempt OS detection if at least one open and one closed TCP port are found
- `nmap 192.168.1.1 -O --osscan-guess` — Makes Nmap guess more aggressively
- `nmap 192.168.1.1 -O --max-os-tries 1` — Set the maximum number of OS detection tries against a target

## Timing and Performance
- `nmap 192.168.1.1 -T0` — Paranoid; slowest, best for IDS evasion
- `nmap 192.168.1.1 -T1` — Sneaky; slow IDS evasion
- `nmap 192.168.1.1 -T2` — Polite; slows down to use less bandwidth and fewer target resources
- `nmap 192.168.1.1 -T3` — Normal; default speed
- `nmap 192.168.1.1 -T4` — Aggressive; faster, assumes reliable network
- `nmap 192.168.1.1 -T5` — Insane; fastest, may miss results on slow networks
- `nmap 192.168.1.1 --min-rate 1000` — Send packets no slower than 1000 per second
- `nmap 192.168.1.1 --max-retries 1` — Reduce retransmissions for faster scans

## NSE Scripts
- `nmap 192.168.1.1 -sC` — Scan with default NSE scripts
- `nmap 192.168.1.1 --script default` — Same as above
- `nmap 192.168.1.1 --script=banner` — Scan with a single script (banner)
- `nmap 192.168.1.1 --script=http*` — Scan with wildcard (all http scripts)
- `nmap 192.168.1.1 --script=http,banner` — Run multiple scripts
- `nmap 192.168.1.1 --script "not intrusive"` — Run default scripts, exclude intrusive ones
- `nmap 192.168.1.1 --script vuln` — Run vulnerability detection scripts
- `nmap 192.168.1.1 --script smb-vuln*` — Check for SMB vulnerabilities
- `nmap 192.168.1.1 --script-args [key=value]` — Pass arguments to scripts
- `nmap --script-updatedb` — Update the NSE script database

## Firewall / IDS Evasion and Spoofing
- `nmap 192.168.1.1 -f` — Fragment packets into tiny IP packets; harder for packet filters
- `nmap 192.168.1.1 -mtu 32` — Set a custom MTU offset size (must be multiple of 8)
- `nmap -D 192.168.1.100,192.168.1.101,ME 192.168.1.1` — Decoy scan; use fake source IPs to obscure origin
- `nmap -D RND:10 192.168.1.1` — Decoy scan with 10 random source IPs
- `nmap 192.168.1.1 -S 192.168.1.200` — Spoof source IP address (requires --send-eth or root)
- `nmap 192.168.1.1 -e eth0` — Use a specific network interface
- `nmap 192.168.1.1 -g 53` — Use a specific source port (e.g., DNS port 53 to evade firewalls)
- `nmap 192.168.1.1 --source-port 53` — Same as above
- `nmap 192.168.1.1 --data-length 25` — Append random data to packets to evade detection
- `nmap 192.168.1.1 --randomize-hosts` — Randomize target host scan order
- `nmap 192.168.1.1 --badsum` — Send packets with incorrect checksums (for testing IDS/IPS response)
- `nmap 192.168.1.1 --proxies socks4://proxy:1080` — Route traffic through SOCKS4 or HTTP proxy

## Output
- `nmap 192.168.1.1 -oN output.txt` — Normal output to a file
- `nmap 192.168.1.1 -oX output.xml` — XML output to a file
- `nmap 192.168.1.1 -oG output.gnmap` — Grepable output to a file
- `nmap 192.168.1.1 -oA output` — Output in all formats (normal, XML, grepable)
- `nmap 192.168.1.1 -v` — Increase verbosity
- `nmap 192.168.1.1 -vv` — Increase verbosity further
- `nmap 192.168.1.1 --reason` — Display the reason a port is in a particular state
- `nmap 192.168.1.1 --open` — Only show open ports
- `nmap 192.168.1.1 --packet-trace` — Show all packets sent and received
- `nmap 192.168.1.1 --iflist` — Print host interfaces and routes (for debugging)
- `nmap 192.168.1.1 --resume output.gnmap` — Resume an aborted scan from a grepable file

## Miscellaneous Nmap Flags
- `nmap -6 [IPv6_address]` — Enable IPv6 scanning
- `nmap -h` — Display help summary
- `nmap --version` — Display Nmap version number
- `nmap --privileged` — Assume that the user is fully privileged
- `nmap --unprivileged` — Assume the user lacks raw socket privileges

## Other Useful Nmap Commands

**Full Comprehensive Scan**
```bash
nmap -sS -sV -sC -O -A -p- 192.168.1.1
```

**Fast Network Sweep**
```bash
nmap -sn 192.168.1.0/24
```

**Scan for Open Web Ports**
```bash
nmap -p 80,443,8080,8443 192.168.1.0/24
```

**Detect Heartbleed Vulnerability**
```bash
nmap -sV --script ssl-heartbleed 192.168.1.1
```

**Check for SMB Vulnerabilities (EternalBlue)**
```bash
nmap --script smb-vuln-ms17-010 -p 445 192.168.1.1
```

**Banner Grabbing**
```bash
nmap -sV --script=banner 192.168.1.1
```

**HTTP Enumeration**
```bash
nmap --script http-enum -p 80,443 192.168.1.1
```

**DNS Brute Force**
```bash
nmap --script dns-brute example.com
```

**Export Results and View in Browser**
```bash
nmap -oX scan.xml 192.168.1.0/24 && xsltproc scan.xml -o scan.html
```
