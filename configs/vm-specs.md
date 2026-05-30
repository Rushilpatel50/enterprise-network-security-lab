# VM Specifications

| VM             | OS                  | RAM  | CPU | Disk | VMnet (NIC)               |
|----------------|---------------------|------|-----|------|---------------------------|
| pfSense        | pfSense CE 2.7      | 1 GB | 1   | 20GB | VMnet0,2,3,4,5 (5 NICs)   |
| Kali Linux     | Kali 2024.x         | 2 GB | 2   | 40GB | VMnet5                    |
| Ubuntu DVWA    | Ubuntu Server 22.04 | 1.5GB| 1   | 20GB | VMnet4                    |
| User Machine   | Ubuntu Desktop 22.04| 2 GB | 1   | 30GB | VMnet3 (optional)         |

## VMnet to VLAN Mapping
- VMnet0  → WAN (bridged to real NIC)
- VMnet2  → VLAN 10 Management (10.10.10.0/24)
- VMnet3  → VLAN 20 Users      (10.20.20.0/24)
- VMnet4  → VLAN 30 DMZ        (10.30.30.0/24)
- VMnet5  → VLAN 99 Attacker   (10.99.99.0/24)
