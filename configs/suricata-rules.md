# Suricata IDS/IPS Configuration

## Deployment
- Installed on:     pfSense CE 2.7.x
- Mode:             IDS (Phase 7-8), IPS (Phase 9)
- Monitored interfaces: ATTACKER (em4), DMZ (em3)
- Ruleset:          Emerging Threats Open (free)

## Enabled Rule Categories
| Category                    | Detects                              |
|-----------------------------|--------------------------------------|
| emerging-scan.rules         | Nmap, port scans, host discovery     |
| emerging-web_server.rules   | Web server attacks, path traversal   |
| emerging-sql.rules          | SQL injection, SQLMap                |
| emerging-exploit.rules      | CVE exploits, shellcode              |
| emerging-user_agents.rules  | Nikto, SQLMap, scanner user agents   |
| emerging-malware.rules      | Malware communication                |
| emerging-dos.rules          | DoS/DDoS patterns                    |
| emerging-policy.rules       | Protocol violations                  |
| emerging-shellcode.rules    | Shellcode in payloads (DMZ only)     |

## Custom Rules Written
| SID     | Description                          |
|---------|--------------------------------------|
| 9000001 | Custom: Nmap scan threshold detect   |
| 9000002 | Custom: SQLMap user agent detect     |
| 9000003 | Custom: Nikto user agent detect      |

## Log Locations (on pfSense)
- Alerts:    /var/log/suricata/*/alerts.log
- EVE JSON:  /var/log/suricata/suricata_attacker.json
             /var/log/suricata/suricata_dmz.json

## IPS Mode Switch (Phase 9)
Services → Suricata → Interfaces → Edit → Block Offenders: ✅