# pfSense Interface Plan

| Interface | VLAN ID | Subnet          | Gateway IP     | Purpose                          |
|-----------|---------|-----------------|----------------|----------------------------------|
| WAN       | —       | DHCP from host  | Host NAT GW    | Internet access (pfSense only)   |
| LAN       | VLAN 10 | 10.10.10.0/24   | 10.10.10.1     | Management — admins, monitoring  |
| OPT1      | VLAN 20 | 10.20.20.0/24   | 10.20.20.1     | Users — workstations             |
| OPT2      | VLAN 30 | 10.30.30.0/24   | 10.30.30.1     | DMZ — DVWA web server            |
| OPT3      | VLAN 99 | 10.99.99.0/24   | 10.99.99.1     | Attacker — Kali Linux (isolated) |

## Key Security Rules (planned)
- Attacker VLAN 99: can only reach DMZ VLAN 30 on ports 80/443
- Attacker VLAN 99: BLOCKED from VLAN 10, 20, and WAN
- DMZ VLAN 30: BLOCKED from initiating connections to VLAN 10 and 20
- Users VLAN 20: can reach DMZ on 80/443, BLOCKED from VLAN 10
- Management VLAN 10: can reach all VLANs for admin purposes

## Confirmed Interface Assignments (Post-Install)

| Interface | em#  | VMnet  | IP Assigned   | Renamed To  |
|-----------|------|--------|---------------|-------------|
| WAN       | em0  | VMnet0 | DHCP (real)   | WAN         |
| LAN       | em1  | VMnet2 | 10.10.10.1    | MANAGEMENT  |
| OPT1      | em2  | VMnet3 | 10.20.20.1    | USERS       |
| OPT2      | em3  | VMnet4 | 10.30.30.1    | DMZ         |
| OPT3      | em4  | VMnet5 | 10.99.99.1    | ATTACKER    |

WebGUI URL (from Management):  http://10.10.10.1
WebGUI URL (from Attacker/Kali): http://10.99.99.1
Admin password: [stored in your password manager]