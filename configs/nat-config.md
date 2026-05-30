# NAT Configuration

## Outbound NAT Rules (Manual Mode)

| Interface | Source          | Translation | Status  |
|-----------|-----------------|-------------|---------|
| WAN       | 10.10.10.0/24   | WAN address | Active  |
| WAN       | 10.20.20.0/24   | WAN address | Active  |
| WAN       | 10.30.30.0/24   | WAN address | Active  |
| WAN       | 10.99.99.0/24   | WAN address | DELETED |

## Security note
VLAN 99 (attacker) is blocked from internet at TWO layers:
1. NAT rule deleted — no address translation possible
2. Firewall rules (Phase 5) — explicit block to WAN