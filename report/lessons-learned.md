# Lessons Learned

## Technical Lessons

### 1. Rule order is the most common firewall misconfiguration
Building this lab made firewall rule ordering intuitive in a way that
reading documentation never could. Seeing packets actually pass when rules
were in the wrong order — then watching them block after reordering —
makes the concept permanent.

### 2. Defence-in-depth requires truly independent layers
Deleting the NAT rule for VLAN 99 felt redundant when firewall rules
already blocked internet access. The Phase 10 validation proved why it
matters — the two controls operate at different layers (routing vs policy)
and neither depends on the other being correctly configured.

### 3. IDS before IPS is non-negotiable in production
Running Suricata in IDS mode during Phase 8 revealed which legitimate
traffic patterns resembled attacks. Had IPS been enabled from the start,
some legitimate DVWA functionality would have been blocked before the
rules were tuned. Real SOC teams follow this exact sequence.

### 4. Custom detection rules demonstrate deeper knowledge
The three custom Suricata rules (Nmap, SQLMap, Nikto detection) fired
alongside the Emerging Threats rules — but with a CUSTOM LAB prefix that
made them instantly recognisable in logs. Writing detection logic from
scratch requires understanding both the attack pattern and Suricata rule
syntax, which is a different skill from enabling pre-built rulesets.

### 5. Wireshark confirms what logs claim
Suricata said SQLMap was detected. The pfSense log said VLAN 99 packets
were blocked. Wireshark independently confirmed both — SQLMap HTTP
requests visible in the PCAP, blocked pings showing SYN packets with no
responses. Cross-correlating three independent evidence sources is how
real incident response works.

## What I Would Do Differently

### Add a SIEM (Security Onion or Splunk Free)
Forwarding Suricata EVE JSON to a SIEM would enable timeline correlation,
dashboard visualisation, and alert aggregation across both monitored
interfaces. This is the natural next step for this lab.

### Test higher DVWA security levels
All attacks ran against DVWA security level Low. Re-running against Medium
and High would demonstrate how input validation, WAF rules, and prepared
statements defeat the same attacks that succeeded at Low — showing the
difference between vulnerable and hardened application code.

### Add a honeypot on the Management VLAN
A honeypot at 10.10.10.50 would generate high-fidelity alerts on any
successful VLAN pivot — any connection to that IP is definitionally
malicious since no legitimate service runs there.

## Real-World Applicability

The architecture built in this lab mirrors what enterprise security teams
actually deploy:

- pfSense equivalent: Cisco ASA, Palo Alto NGFW, Fortinet FortiGate
- Suricata equivalent: Snort, Zeek, commercial NGFW inspection engines
- DVWA equivalent: any production web application behind a DMZ
- VLAN segmentation: standard in every enterprise network above 50 users
- The attack sequence: standard PTES/OWASP penetration testing methodology

Every concept practised here has a direct production equivalent.
