# DHCP Configuration

| Interface   | VLAN | Gateway      | DHCP Range                    | Notes                        |
|-------------|------|--------------|-------------------------------|------------------------------|
| MANAGEMENT  | 10   | 10.10.10.1   | 10.10.10.100 – 10.10.10.200   |                              |
| USERS       | 20   | 10.20.20.1   | 10.20.20.100 – 10.20.20.200   |                              |
| DMZ         | 30   | 10.30.30.1   | 10.30.30.100 – 10.30.30.200   | DVWA uses static 10.30.30.10 |
| ATTACKER    | 99   | 10.99.99.1   | 10.99.99.100 – 10.99.99.200   | Kali static lease 10.99.99.10|

# NAT Configuration

Outbound NAT mode: Manual
NAT rules active for: VLAN 10, 20, 30
NAT rule DELETED for: VLAN 99 (attacker has no internet)