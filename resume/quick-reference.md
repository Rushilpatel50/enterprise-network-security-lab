# Project Quick Reference — Before Interviews

## The one-line summary
"I built a 4-VLAN enterprise security lab with pfSense, Suricata IDS/IPS,
and a DVWA target — ran a full attack simulation and documented every
detection with logs, PCAPs, and a validation matrix."

## Numbers to remember
- 4 VLANs: 10 (Mgmt), 20 (Users), 30 (DMZ), 99 (Attacker)
- 5 NICs on pfSense (WAN + 4 internal)
- 23 firewall rules across 4 interfaces
- 8 Emerging Threats rule categories
- 8 attack types simulated
- 12 validation tests — all passed
- 40+ screenshots of evidence

## Key concepts you demonstrated
- Network segmentation and DMZ architecture
- Firewall rule ordering and implicit deny
- Defence-in-depth (NAT + firewall, VMnet + policy)
- IDS vs IPS — detection vs active response
- Attack chain: recon → scan → exploit → document
- Evidence correlation: alerts + logs + PCAPs
- Professional penetration testing documentation

