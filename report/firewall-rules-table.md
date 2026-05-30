# Firewall Rules Reference Table

## Design Principles
- Rules processed top to bottom — first match wins
- All block rules placed BEFORE allow rules for the same destination
- Logging enabled on all block rules for audit trail
- Implicit deny at bottom of every interface (pfSense default)
- Rules applied inbound on the source interface

---

## ATTACKER Interface (VLAN 99) — 7 Rules

| # | Action | Protocol | Source       | Destination   | Port    | Log | Purpose                    |
|---|--------|----------|--------------|---------------|---------|-----|----------------------------|
| 1 | BLOCK  | Any      | ATTACKER net | 10.10.10.0/24 | Any     | ✅  | Prevent pivot to mgmt      |
| 2 | BLOCK  | Any      | ATTACKER net | 10.20.20.0/24 | Any     | ✅  | Prevent pivot to users     |
| 3 | BLOCK  | TCP      | ATTACKER net | 10.99.99.1    | 80,443  | ✅  | Prevent firewall GUI access|
| 4 | PASS   | TCP      | ATTACKER net | 10.30.30.10   | 80      | ✅  | Allow attack on DVWA HTTP  |
| 5 | PASS   | TCP      | ATTACKER net | 10.30.30.10   | 443     | ✅  | Allow attack on DVWA HTTPS |
| 6 | PASS   | UDP      | ATTACKER net | 10.99.99.1    | 53      | ❌  | Allow DNS resolution       |
| 7 | BLOCK  | Any      | ATTACKER net | Any           | Any     | ✅  | Catch-all deny             |

## DMZ Interface (VLAN 30) — 6 Rules

| # | Action | Protocol | Source  | Destination   | Port    | Log | Purpose                        |
|---|--------|----------|---------|---------------|---------|-----|--------------------------------|
| 1 | BLOCK  | Any      | DMZ net | 10.10.10.0/24 | Any     | ✅  | DMZ cannot reach mgmt          |
| 2 | BLOCK  | Any      | DMZ net | 10.20.20.0/24 | Any     | ✅  | DMZ cannot reach users         |
| 3 | BLOCK  | Any      | DMZ net | 10.99.99.0/24 | Any     | ✅  | DMZ cannot reach attacker      |
| 4 | PASS   | TCP      | DMZ net | Any           | 80,443  | ❌  | Allow internet for updates     |
| 5 | PASS   | UDP      | DMZ net | 10.30.30.1    | 53      | ❌  | Allow DNS resolution           |
| 6 | BLOCK  | Any      | DMZ net | Any           | Any     | ✅  | Catch-all deny                 |

## USERS Interface (VLAN 20) — 5 Rules

| # | Action | Protocol | Source    | Destination   | Port    | Log | Purpose                      |
|---|--------|----------|-----------|---------------|---------|-----|------------------------------|
| 1 | BLOCK  | Any      | USERS net | 10.10.10.0/24 | Any     | ✅  | Users cannot reach mgmt      |
| 2 | BLOCK  | Any      | USERS net | 10.99.99.0/24 | Any     | ✅  | Users cannot reach attacker  |
| 3 | PASS   | TCP      | USERS net | 10.30.30.10   | 80,443  | ❌  | Allow browsing DVWA          |
| 4 | PASS   | TCP      | USERS net | Any           | 80,443  | ❌  | Allow internet browsing      |
| 5 | PASS   | UDP      | USERS net | 10.20.20.1    | 53      | ❌  | Allow DNS resolution         |

## MANAGEMENT Interface (VLAN 10) — 1 Rule

| # | Action | Protocol | Source         | Destination | Port | Log | Purpose                  |
|---|--------|----------|----------------|-------------|------|-----|--------------------------|
| 1 | PASS   | Any      | MANAGEMENT net | Any         | Any  | ❌  | Full admin access        |

---

## Rule Order — Why It Matters

Example: ATTACKER interface rules 1 and 4

If rule 4 (PASS → 10.30.30.10 port 80) came BEFORE rule 1
(BLOCK → 10.10.10.0/24), and Kali sent a packet to 10.10.10.1 on port 80,
pfSense would match rule 4 first (destination is not 10.30.30.10 so it
would not match) — but if rule 4 was written as PASS to ANY destination
on port 80, management would be reachable. Block rules must always precede
allow rules for overlapping destinations.
