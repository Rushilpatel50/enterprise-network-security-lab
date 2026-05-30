# Final Validation Results

## Network Segmentation Validation

| Test # | From         | To              | Expected | Actual | Status |
|--------|--------------|-----------------|----------|--------|--------|
| 1      | Kali 99      | DVWA :80        | PASS     | PASS   | ✅     |
| 2      | Kali 99      | DVWA :443       | PASS     | PASS   | ✅     |
| 3      | Kali 99      | DNS 10.99.99.1  | PASS     | PASS   | ✅     |
| 4      | Kali 99      | Mgmt 10.10.10.1 | BLOCK    | BLOCK  | ✅     |
| 5      | Kali 99      | Users 10.20.20.1| BLOCK    | BLOCK  | ✅     |
| 6      | Kali 99      | Internet        | BLOCK    | BLOCK  | ✅     |
| 7      | Kali 99      | pfSense WebGUI  | BLOCK    | BLOCK  | ✅     |
| 8      | Kali 99      | DVWA :22,:3306  | BLOCK    | BLOCK  | ✅     |
| 9      | DVWA 30      | Internet        | PASS     | PASS   | ✅     |
| 10     | DVWA 30      | Mgmt 10.10.10.1 | BLOCK    | BLOCK  | ✅     |
| 11     | DVWA 30      | Users 10.20.20.1| BLOCK    | BLOCK  | ✅     |
| 12     | DVWA 30      | Attacker 99     | BLOCK    | BLOCK  | ✅     |

## IDS/IPS Validation

| Test | Action                  | Expected Result         | Actual | Status |
|------|-------------------------|-------------------------|--------|--------|
| A    | Nmap scan in IDS mode   | Alert generated, not blocked | Alert ✅ | ✅ |
| B    | SQLMap in IDS mode      | Alert generated, attack succeeds | Alert ✅ | ✅ |
| C    | Nikto in IDS mode       | Alert generated, scan completes | Alert ✅ | ✅ |
| D    | Nmap scan in IPS mode   | Alert + Kali IP blocked  | Blocked ✅ | ✅ |
| E    | Manual block removal    | Access restored to DVWA  | Restored ✅ | ✅ |

## VLAN Architectural Isolation

| Test | Description                              | Result | Status |
|------|------------------------------------------|--------|--------|
| F    | Firewall disabled — Kali still blocked   | Blocked by VMnet separation | ✅ |

## Evidence Completeness

| Evidence Type      | Files Present | Status |
|--------------------|---------------|--------|
| PCAPs              | 4 files       | ✅     |
| Suricata alerts    | 2+ files      | ✅     |
| Firewall logs      | 1+ files      | ✅     |
| Kali attack output | 5+ files      | ✅     |
| Screenshots        | 40+ images    | ✅     |
| Config docs        | 5 files       | ✅     |
| Report drafts      | 4 files       | ✅     |

# Enterprise Network Security Lab — Project Report

**Author:** [Your Name]
**Date:** [Date]
**Lab Platform:** VMware Workstation Pro 17
**Duration:** [X hours/days]

---

## Executive Summary

This project demonstrates the design, implementation, and validation of a
segmented enterprise network security lab. The lab simulates a realistic
corporate network architecture including a firewall, DMZ, isolated VLANs,
IDS/IPS monitoring, and controlled attack simulation.

The goal was to build a practical understanding of how enterprise security
teams use network segmentation, firewall policy, and intrusion detection to
defend against common web application attacks — and to produce documented
evidence suitable for a professional security portfolio.

---

## Objectives

1. Design and implement a 4-VLAN segmented network using pfSense
2. Deploy a vulnerable web application in an isolated DMZ
3. Configure Suricata IDS/IPS with Emerging Threats rulesets
4. Simulate realistic attacks from an isolated attacker network
5. Detect, log, and analyze all attack activity
6. Validate all security controls with documented test results

---

## Environment

### Virtual Infrastructure
- Hypervisor: VMware Workstation Pro 17 (Windows/Linux host)
- Host RAM used: approximately 6.5 GB across all running VMs

### Virtual Machines

| VM           | OS                  | Role            | IP            |
|--------------|---------------------|-----------------|---------------|
| pfSense      | pfSense CE 2.7      | Firewall/Router | 10.x.x.1 each|
| Kali Linux   | Kali 2024.x         | Attacker        | 10.99.99.10   |
| Ubuntu DVWA  | Ubuntu Server 22.04 | Target/DMZ      | 10.30.30.10   |
| Admin VM     | Ubuntu Desktop      | Management      | 10.10.10.x    |

---

## Network Design Decisions

### Why pfSense?
pfSense is a production-grade open source firewall used by thousands of
enterprises. Running it as a VM gives full access to its WebGUI, CLI,
package ecosystem (including Suricata), and firewall rule engine — identical
to a physical deployment.

### Why VMware Host-only Networks Instead of VLANs?
802.1q VLAN tagging requires a managed switch. VMware Host-only networks
provide the same isolation guarantee in software — each VMnet is a separate
Layer 2 broadcast domain that cannot communicate with others without
explicitly routing through pfSense. This architectural isolation was validated
in Phase 10 by disabling the pfSense firewall and confirming cross-VLAN
communication was still impossible.

### Why a DMZ for DVWA?
A DMZ (De-Militarised Zone) is the standard architecture for hosting
internet-facing services. It sits between the untrusted internet and the
trusted internal network. Even if an attacker fully compromises a DMZ server,
firewall rules prevent lateral movement into internal VLANs. This mirrors
how real enterprises host web applications, APIs, and public-facing servers.

### Why NAT Deletion for VLAN 99?
Firewall rules can be misconfigured. Deleting the NAT rule for the attacker
VLAN provides a second, independent layer of internet isolation that cannot
be bypassed by a firewall rule misconfiguration. This demonstrates the
principle of defence-in-depth — multiple independent controls protecting
the same asset.

---

## Implementation Summary

### Phase 1–2: Planning and Platform Setup
Defined VLAN architecture, subnet design, and VM specifications before
touching any software. Created VMware virtual networks with DHCP disabled
on internal segments.

### Phase 3–4: pfSense Installation and Network Config
Installed pfSense CE, assigned all 5 interfaces, configured DHCP on all
internal VLANs, and set manual outbound NAT with VLAN 99 excluded.

### Phase 5: Firewall Rules
Implemented 23 explicit firewall rules across 4 interfaces. Key design
decisions: block rules before allow rules on each interface, explicit logging
on all deny rules, and a final catch-all block on ATTACKER and DMZ interfaces.

### Phase 6: DVWA Deployment
Deployed Ubuntu Server 22.04 with Apache, MariaDB, PHP, and DVWA in the
DMZ. Assigned static IP 10.30.30.10. Set DVWA security level to Low for
maximum attack surface during simulation.

### Phase 7: Suricata IDS/IPS
Installed Suricata via pfSense Package Manager. Configured monitoring on
both ATTACKER and DMZ interfaces. Enabled 8 Emerging Threats rule categories.
Wrote 3 custom detection rules (SID 9000001–9000003) targeting Nmap, SQLMap,
and Nikto specifically.

### Phase 8: Attack Simulation
Executed a realistic penetration testing workflow: reconnaissance (Nmap),
web scanning (Nikto), manual SQL injection, automated SQL injection (SQLMap),
reflected and stored XSS, and documented blocked VLAN pivot attempts.

### Phase 9–10: Detection Analysis and Validation
Correlated Suricata alerts with Wireshark PCAPs and pfSense firewall logs.
Validated IPS blocking mode. Confirmed all 12 network segmentation tests
passed. Exported complete evidence package.

---

## Results

### Security Controls — All Validated ✅

| Control                          | Mechanism            | Validated |
|----------------------------------|----------------------|-----------|
| VLAN 99 → VLAN 10 blocked        | pfSense firewall     | ✅        |
| VLAN 99 → VLAN 20 blocked        | pfSense firewall     | ✅        |
| VLAN 99 → Internet blocked       | NAT deletion         | ✅        |
| VLAN 30 → VLAN 10 blocked        | pfSense firewall     | ✅        |
| VLAN 30 → VLAN 20 blocked        | pfSense firewall     | ✅        |
| Nmap detected by Suricata        | ET SCAN rules        | ✅        |
| Nikto detected by Suricata       | Custom rule 9000003  | ✅        |
| SQLMap detected by Suricata      | Custom rule 9000002  | ✅        |
| SQL injection detected           | ET SQL rules         | ✅        |
| XSS detected                     | ET WEB_SERVER rules  | ✅        |
| IPS blocking mode functional     | Suricata block list  | ✅        |
| VLAN isolation architectural     | VMware Host-only     | ✅        |

---

## Lessons Learned

See `report/lessons-learned.md` for full discussion.

---

## References

- pfSense Documentation: docs.netgate.com
- Suricata Documentation: suricata.io/docs
- Emerging Threats Rules: rules.emergingthreats.net
- DVWA: github.com/digininja/DVWA
- OWASP Top 10: owasp.org/Top10
