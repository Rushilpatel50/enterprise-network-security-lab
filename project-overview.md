# Project Overview

## Enterprise Network Security Lab

### Summary
A fully functional enterprise network security lab built in VMware Workstation
demonstrating firewall segmentation, DMZ architecture, IDS/IPS monitoring,
and controlled attack simulation with documented detection evidence.

### Objectives
- Design and implement a 4-VLAN segmented network using pfSense
- Deploy a vulnerable web application (DVWA) in an isolated DMZ
- Configure Suricata IDS/IPS with Emerging Threats rulesets
- Simulate realistic attacks from an isolated attacker network
- Detect, log, and analyze all attack activity
- Validate all security controls with a documented test matrix

### Key Results
- 23 firewall rules across 4 interfaces — all validated
- 8 attack types simulated and detected by Suricata
- 12-point connectivity validation matrix — all tests passed
- VLAN isolation confirmed at architectural level (independent of firewall)
- IDS and IPS modes both tested and documented

### Skills Demonstrated
- Network segmentation and DMZ architecture
- pfSense firewall rule design and ordering
- Suricata IDS/IPS deployment and custom rule authoring
- Penetration testing workflow (Nmap, Nikto, SQLMap, manual exploitation)
- Evidence collection and correlation (alerts + logs + PCAPs)
- Professional security documentation

### Lab Architecture
| VLAN | Name       | Subnet          | Role                        |
|------|------------|-----------------|-----------------------------|
| 10   | Management | 10.10.10.0/24   | Admin access and monitoring |
| 20   | Users      | 10.20.20.0/24   | User workstations           |
| 30   | DMZ        | 10.30.30.0/24   | DVWA vulnerable web server  |
| 99   | Attacker   | 10.99.99.0/24   | Kali Linux — isolated       |

### Tools Used
pfSense · Suricata · Kali Linux · DVWA · Nmap · Nikto · SQLMap · Wireshark · VMware Workstation

### Full Report
See `report/enterprise-network-security-report.md` for the complete
technical report including design decisions, implementation details,
and validated results.
