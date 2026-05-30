# Suricata Detection Analysis Notes

## Alert Summary
- Total alerts generated:    5507
- Unique signatures fired:   25
- Highest priority alerts:   Priority 1 = SQLi, XSS, exploit attempts
- Custom rules triggered:    SID 9000001, 9000002, 9000003

## Key Observations

### Nmap Detection
Suricata detected Nmap within seconds of scan start. The ET SCAN rules
matched on Nmap's default TCP SYN scan pattern and its HTTP script
user-agent string. Custom rule SID 9000001 also fired on the SYN
threshold (10 SYNs in 5 seconds).

### SQLMap Detection  
Two detection layers fired simultaneously:
1. ET SQL rules matched on SQL syntax patterns in HTTP parameters
2. Custom rule SID 9000002 matched on SQLMap's default user-agent string
The tool was detected before it completed its first injection test.

### XSS Detection
ET WEB_SERVER rules matched on script tag patterns in HTTP POST bodies.
Reflected and stored XSS attempts both generated alerts.

### Firewall Segmentation
All 4 blocked access attempts (MGMT, USERS, internet, WebGUI) were
logged in pfSense firewall logs. Zero packets reached their
unintended destinations — confirming VLAN segmentation is airtight.

### IPS Mode Validation
When switched to IPS mode, Suricata blocked Kali's IP after detecting
the Nmap scan. Subsequent HTTP requests from the same IP were dropped
even though port 80 was explicitly allowed by firewall rules —
demonstrating defence-in-depth (firewall + IDS/IPS layered).

## IDS vs IPS Comparison
| Mode | Attack reaches target | Alert generated | IP blocked |
|------|-----------------------|-----------------|------------|
| IDS  | ✅ Yes                | ✅ Yes          | ❌ No      |
| IPS  | ❌ No (after detect)  | ✅ Yes          | ✅ Yes     |
