# Resume Bullet Points — Enterprise Network Security Lab

## Option A — Concise bullets (best for most resumes)

- Designed and deployed a segmented enterprise network lab in VMware Workstation
  with pfSense firewall, 4 isolated VLANs, DMZ architecture, and manual NAT
  policy enforcing dual-layer internet isolation on the attacker network

- Configured 23 ordered firewall rules across 4 interfaces enforcing strict
  inter-VLAN segmentation; validated all rules with a 12-test connectivity
  matrix producing zero false results

- Deployed Suricata IDS/IPS on pfSense monitoring two interfaces with Emerging
  Threats Open ruleset across 8 attack categories; authored 3 custom detection
  rules targeting Nmap, SQLMap, and Nikto tool signatures

- Executed a complete penetration testing workflow against DVWA — Nmap
  reconnaissance, Nikto web scanning, manual and automated SQL injection,
  and XSS exploitation — with every attack detected by Suricata and
  captured in Wireshark PCAPs

- Correlated IDS alerts, firewall logs, and Wireshark packet captures across
  a 12-attack simulation session; demonstrated IPS auto-blocking and confirmed
  architectural VLAN isolation independent of firewall policy


## Option B — Impact-focused bullets (stronger for SOC/analyst roles)

- Built a 4-VLAN enterprise security lab simulating DMZ architecture, firewall
  segmentation, and IDS/IPS monitoring — reproducing the defensive stack used
  in production corporate environments

- Detected and logged 8 attack types (port scanning, web enumeration, SQL
  injection, XSS) using Suricata with custom-authored detection rules;
  demonstrated both IDS visibility and IPS automated response modes

- Validated defence-in-depth by confirming VLAN isolation held at the
  architectural layer when firewall policy was deliberately disabled —
  proving segmentation does not depend on a single control

- Produced a complete evidence package — Suricata EVE JSON logs, pfSense
  firewall exports, Wireshark PCAPs, and 40+ screenshots — following the
  documentation standard used in professional penetration testing sign-off


## Which bullets to use where

| Role                        | Use these bullets          |
|-----------------------------|----------------------------|
| SOC Analyst                 | A3, A5, B2, B4             |
| Network Security Engineer   | A1, A2, B1, B3             |
| Penetration Tester          | A4, B2, B4                 |
| Junior Security Engineer    | A1, A3, A4, A5             |
| Security Internship         | B1, B2, B3 (concise)       |
