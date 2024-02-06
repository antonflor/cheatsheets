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
- `nmap 192.168.1.1`: Scan a single IP
- `nmap 192.168.1.1 192.168.2.1`: Scan specific IPs
- `nmap 192.168.1.1-254`: Scan a range
- `nmap scanme.nmap.org`: Scan a domain
- `nmap 192.168.1.0/24`: Scan using CIDR notation
- `-iL nmap -iL targets.txt`: Scan targets from a file
- `-iR nmap -iR 100`: Scan 100 random hosts
- `-exclude nmap -exclude 192.168.1.1`: Exclude listed hosts

## Nmap Scan Techniques
- `-sS nmap 192.168.1.1 -sS`: TCP SYN port scan (Default)
- `-sT nmap 192.168.1.1 -sT`: TCP connect port scan (Default without root privilege)
- `-sU nmap 192.168.1.1 -sU`: UDP port scan
- `-sA nmap 192.168.1.1 -sA`: TCP ACK port scan
- `-sW nmap 192.168.1.1 -sW`: TCP Window port scan
- `-sM nmap 192.168.1.1 -sM`: TCP Maimon port scan

## Host Discovery
- `-sL nmap 192.168.1.1-3 -sL`: No Scan. List targets only
- `-sn nmap 192.168.1.1/24 -sn`: Disable port scanning. Host discovery only.
- `-Pn nmap 192.168.1.1-5 -Pn`: Disable host discovery. Port scan only.

## Port Specification
- `-p nmap 192.168.1.1 -p 21`: Port scan for port x
- `-p nmap 192.168.1.1 -p 21-100`: Port range
- `-p nmap 192.168.1.1 -p U:53,T:21-25,80`: Port scan multiple TCP and UDP ports
- `-p nmap 192.168.1.1 -p-`: Port scan all ports
- `-p nmap 192.168.1.1 -p http,https`: Port scan from service name

## Service and Version Detection
- `-sV nmap 192.168.1.1 -sV`: Attempts to determine the version of the service running on port
- `-sV -version-intensity nmap 192.168.1.1 -sV -version-intensity 8`: Intensity level 0 to 9. Higher number increases possibility of correctness
- `-sV -version-light nmap 192.168.1.1 -sV -version-light`: Enable light mode. Lower possibility of correctness. Faster
- `-sV -version-all nmap 192.168.1.1 -sV -version-all`: Enable intensity level 9. Higher possibility of correctness. Slower
- `-A nmap 192.168.1.1 -A`: Enables OS detection, version detection, script scanning, and traceroute

## OS Detection
- `-O nmap 192.168.1.1 -O`: Remote OS detection using TCP/IP stack fingerprinting
- `-O -osscan-limit nmap 192.168.1.1 -O -osscan-limit`: If at least one open and one closed TCP port are not found it will not try OS detection against host
- `-O -osscan-guess nmap 192.168.1.1 -O -osscan-guess`: Makes Nmap guess more aggressively
- `-O -max-os-tries nmap 192.168.1.1 -O -max-os-tries 1`: Set the maximum number x of OS detection tries against a target

## Timing and Performance
- `-T0 nmap 192.168.1.1 -T0`: Paranoid (0) Intrusion Detection System evasion
- `-T1 nmap 192.168.1.1 -T1`: Sneaky (1) Intrusion Detection System evasion
- `-T2 nmap 192.168.1.1 -T2`: Polite (2) slows down the scan to use less bandwidth and use less target machine resources
- `-T3 nmap 192.168.1.1 -T3`: Normal (3) which is default speed
- `-T4 nmap 192.168.1.1 -T4`: Aggressive (4) speeds scans; assumes you are on a reasonably fast and reliable network
- `-T5 nmap 192.168.1.1 -T5`: Insane (5) speeds scan; assumes you are on an extraordinarily fast network

## NSE Scripts
- `-sC nmap 192.168.1.1 -sC`: Scan with default NSE scripts. Considered useful for discovery and safe
- `-script default nmap 192.168.1.1 -script default`: Scan with default NSE scripts. Considered useful for discovery and safe
- `-script nmap 192.168.1.1 -script=banner`: Scan with a single script. Example banner
- `-script nmap 192.168.1.1 -script=http*`: Scan with a wildcard. Example http
- `-script nmap 192.168.1.1 -script=http,banner`: Scan with two scripts. Example http and banner
- `-script nmap 192.168.1.1 -script "not intrusive"`: Scan default, but remove intrusive scripts

## Firewall / IDS Evasion and Spoofing
- `-f nmap 192.168.1.1 -f`: Requested scan (including ping scans) use tiny fragmented IP packets. Harder for packet filters
- `-mtu nmap 192.168.1.1 -mtu 32`: Set your own offset size
- `-D nmap -D 192
