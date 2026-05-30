# Firewall Rules Design (Phase 1 Planning)

## Rule order matters — pfSense reads top to bottom, first match wins.

### VLAN 99 (Attacker) Rules
| # | Action | Source       | Destination  | Port    | Reason                        |
|---|--------|--------------|--------------|---------|-------------------------------|
| 1 | ALLOW  | 10.99.99.0/24| 10.30.30.10  | 80,443  | Attack DVWA only              |
| 2 | BLOCK  | 10.99.99.0/24| 10.10.10.0/24| ANY     | No access to management       |
| 3 | BLOCK  | 10.99.99.0/24| 10.20.20.0/24| ANY     | No access to users            |
| 4 | BLOCK  | 10.99.99.0/24| ANY          | ANY     | No internet, no other access  |

### VLAN 30 (DMZ) Rules
| # | Action | Source       | Destination  | Port    | Reason                        |
|---|--------|--------------|--------------|---------|-------------------------------|
| 1 | BLOCK  | 10.30.30.0/24| 10.10.10.0/24| ANY     | DMZ cannot reach management   |
| 2 | BLOCK  | 10.30.30.0/24| 10.20.20.0/24| ANY     | DMZ cannot reach users        |
| 3 | ALLOW  | 10.30.30.0/24| ANY          | 80,443  | Web server can respond        |

### VLAN 20 (Users) Rules
| # | Action | Source       | Destination  | Port    | Reason                        |
|---|--------|--------------|--------------|---------|-------------------------------|
| 1 | BLOCK  | 10.20.20.0/24| 10.10.10.0/24| ANY     | Users cannot reach management |
| 2 | ALLOW  | 10.20.20.0/24| 10.30.30.0/24| 80,443  | Users browse DMZ web server   |
| 3 | ALLOW  | 10.20.20.0/24| ANY          | ANY     | Normal internet browsing      |

### VLAN 10 (Management) Rules
| # | Action | Source       | Destination  | Port    | Reason                        |
|---|--------|--------------|--------------|---------|-------------------------------|
| 1 | ALLOW  | 10.10.10.0/24| ANY          | ANY     | Admins need full access       |

# Firewall Rules — Final Configuration

## Rule Order Logic
- pfSense reads top to bottom, first match wins
- Block rules come BEFORE allow rules for the same destination
- Explicit block rules are logged so attacks appear in firewall logs
- Implicit deny covers anything not explicitly allowed

## ATTACKER (VLAN 99) Rules
| # | Action | Source       | Destination   | Port    | Logged |
|---|--------|--------------|---------------|---------|--------|
| 1 | BLOCK  | ATTACKER net | 10.10.10.0/24 | Any     | Yes    |
| 2 | BLOCK  | ATTACKER net | 10.20.20.0/24 | Any     | Yes    |
| 3 | BLOCK  | ATTACKER net | 10.99.99.1    | 80,443  | Yes    |
| 4 | PASS   | ATTACKER net | 10.30.30.10   | 80      | Yes    |
| 5 | PASS   | ATTACKER net | 10.30.30.10   | 443     | Yes    |
| 6 | PASS   | ATTACKER net | 10.99.99.1    | 53 UDP  | No     |
| 7 | BLOCK  | ATTACKER net | Any           | Any     | Yes    |

## DMZ (VLAN 30) Rules
| # | Action | Source  | Destination   | Port    | Logged |
|---|--------|---------|---------------|---------|--------|
| 1 | BLOCK  | DMZ net | 10.10.10.0/24 | Any     | Yes    |
| 2 | BLOCK  | DMZ net | 10.20.20.0/24 | Any     | Yes    |
| 3 | BLOCK  | DMZ net | 10.99.99.0/24 | Any     | Yes    |
| 4 | PASS   | DMZ net | Any           | 80,443  | No     |
| 5 | PASS   | DMZ net | 10.30.30.1    | 53 UDP  | No     |
| 6 | BLOCK  | DMZ net | Any           | Any     | Yes    |

## USERS (VLAN 20) Rules
| # | Action | Source    | Destination   | Port    | Logged |
|---|--------|-----------|---------------|---------|--------|
| 1 | BLOCK  | USERS net | 10.10.10.0/24 | Any     | Yes    |
| 2 | BLOCK  | USERS net | 10.99.99.0/24 | Any     | Yes    |
| 3 | PASS   | USERS net | 10.30.30.10   | 80,443  | No     |
| 4 | PASS   | USERS net | Any           | 80,443  | No     |
| 5 | PASS   | USERS net | 10.20.20.1    | 53 UDP  | No     |

## MANAGEMENT (VLAN 10) Rules
| # | Action | Source         | Destination | Port | Logged |
|---|--------|----------------|-------------|------|--------|
| 1 | PASS   | MANAGEMENT net | Any         | Any  | No     |