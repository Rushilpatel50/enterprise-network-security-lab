# Attack vs Detection Evidence Table

| # | Attack | Tool | Source | Target | Suricata Alert | Firewall Log | PCAP File |
|---|--------|------|--------|--------|---------------|--------------|-----------|
| 1 | Host discovery | Nmap -sn | 10.99.99.10 | 10.30.30.0/24 | ET SCAN Nmap host discovery | — | nmap-scan.pcap |
| 2 | Full port scan | Nmap -sV -p- | 10.99.99.10 | 10.30.30.10 | ET SCAN Nmap Version Scan | — | nmap-scan.pcap |
| 3 | Web enumeration | Nmap scripts | 10.99.99.10 | 10.30.30.10 | ET SCAN Nmap NSE User-Agent | — | nmap-scan.pcap |
| 4 | Web vuln scan | Nikto | 10.99.99.10 | 10.30.30.10 | ET WEB_SERVER Nikto Scanner | — | nikto-scan.pcap |
| 5 | Manual SQLi | Browser | 10.99.99.10 | 10.30.30.10 | ET WEB_SERVER SQL Injection | — | sqlmap-test.pcap |
| 6 | Automated SQLi | SQLMap | 10.99.99.10 | 10.30.30.10 | CUSTOM LAB SQLMap detected | — | sqlmap-test.pcap |
| 7 | XSS reflected | Browser | 10.99.99.10 | 10.30.30.10 | ET WEB_SERVER XSS attempt | — | full-session.pcap |
| 8 | XSS stored | Browser | 10.99.99.10 | 10.30.30.10 | ET WEB_SERVER XSS stored | — | full-session.pcap |
| 9 | MGMT VLAN probe | ping | 10.99.99.10 | 10.10.10.1 | — | BLOCKED logged | blocked-intervlan.pcap |
| 10 | USERS VLAN probe | ping | 10.99.99.10 | 10.20.20.1 | — | BLOCKED logged | blocked-intervlan.pcap |
| 11 | Internet access | curl | 10.99.99.10 | 8.8.8.8 | — | BLOCKED (NAT) | blocked-intervlan.pcap |
| 12 | pfSense WebGUI | curl | 10.99.99.10 | 10.99.99.1:80 | — | BLOCKED logged | blocked-intervlan.pcap |