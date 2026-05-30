# 30-Second Recruiter Explanation

## Use this when a recruiter asks "tell me about a project you've worked on" — at a career fair, on a phone screen, or in a LinkedIn message:

- "I built an enterprise network security lab from scratch in VMware —
  four isolated network segments connected through a pfSense firewall,
  with a vulnerable web server in a DMZ and Kali Linux as an attacker
  on a completely separate isolated network.

- I configured all the firewall rules, set up Suricata as an IDS and IPS,
  then ran a full attack simulation — Nmap scanning, SQL injection, XSS —
  and documented every detection with Suricata alerts, firewall logs,
  and Wireshark packet captures.

- The whole thing is on GitHub with a full report and evidence package.
  It's the kind of architecture you'd find in a real enterprise — pfSense
  maps directly to a Cisco ASA or Palo Alto in production."

---

# 2-Minute Technical Explanation

## Use this when an interviewer says "walk me through this project technically":

"The lab has four VLANs — Management, Users, DMZ, and an isolated
Attacker network. Each one is a separate Host-only network in VMware,
which gives you architectural isolation at Layer 2 before any firewall
policy even applies.

pfSense sits in the middle with five NICs — one per segment — running
DHCP, NAT, and the firewall. I configured manual outbound NAT and
deleted the NAT rule for the attacker VLAN entirely, so even if a
firewall rule was misconfigured, the attacker has no path to the internet.
That's defence-in-depth — two independent controls on the same threat.

The firewall has 23 explicit rules. The key design decision is rule
ordering — pfSense reads top to bottom and stops at the first match,
so block rules for sensitive destinations always sit above allow rules.
I put DVWA in the DMZ at a static IP so the attacker rules could
reference a specific host rather than a subnet.

Suricata runs on pfSense monitoring both the attacker and DMZ interfaces.
I enabled eight Emerging Threats rule categories and wrote three custom
rules that detect Nmap, SQLMap, and Nikto specifically by their tool
signatures. Running it in IDS mode first was deliberate — I wanted to
see what legitimate traffic looked like before enabling blocking, which
is exactly how a real SOC team would tune a new IDS deployment.

For the attack simulation I ran the full recon-to-exploitation chain —
Nmap host discovery and port scanning, Nikto web enumeration, manual
SQL injection to extract credentials, SQLMap to automate it, and
reflected and stored XSS. Every attack generated Suricata alerts. I
also ran deliberate blocked attempts toward Management and Users VLANs
to generate firewall log evidence.

In Phase 9 I correlated three evidence sources — Suricata EVE JSON
logs, pfSense firewall exports, and Wireshark PCAPs — for each attack
type. The Wireshark analysis was particularly useful because it showed
the Nmap SYN flood pattern at the packet level and the SQLMap payloads
URL-encoded in the HTTP stream, which explains exactly why those
Suricata rules fire.

The final validation confirmed all 12 connectivity tests produced the
expected result, and I verified the VLAN isolation holds even with the
firewall disabled — which proves the segmentation is architectural,
not just policy-dependent."

--- 

# Interview Q&A

---

Q: What is a DMZ and why did you use one?

A: A DMZ is a network segment that sits between the untrusted internet and
the trusted internal network. It hosts internet-facing services — in this
case a web server — so that if an attacker fully compromises that server,
firewall rules prevent them from pivoting into internal networks. The DVWA
server in my lab sits in VLAN 30. Even with full OS access to that machine,
the firewall blocks any connection it tries to initiate toward Management
or Users VLANs. That containment is the whole point of a DMZ.

---

Q: Explain the difference between IDS and IPS mode in your lab.

A: In IDS mode Suricata detects and logs attacks but lets the traffic
through — the attack reaches DVWA and I can study both the request and
the response. In IPS mode Suricata adds the offending source IP to a
block list and subsequent packets from that IP are dropped at the firewall
level, even if a firewall rule would normally allow them. I ran IDS mode
during the attack simulation so I could capture complete attack sessions,
then switched to IPS mode to validate that blocking works. In a real SOC
you would start with IDS to tune rules and understand your traffic baseline
before enabling blocking, which can cause false-positive outages.

---

Q: Why does rule order matter in a firewall?

A: pfSense processes rules top to bottom and stops at the first match.
If you put an allow rule above a block rule for the same destination,
the allow rule matches first and the block rule never runs — traffic is
permitted even though a block rule exists. In my ATTACKER interface rules
the block rules for Management and Users VLANs sit at positions 1 and 2,
above the allow rules for DVWA at positions 4 and 5. If those were
reversed and someone wrote a broad allow rule, Management would become
reachable. Rule ordering is the most common real-world firewall
misconfiguration.

---

Q: What is defence-in-depth and where did you apply it?

A: Defence-in-depth means layering multiple independent security controls
so that no single failure exposes the asset. I applied it in two places.
First, the attacker VLAN has no internet access enforced at two independent
layers — a deleted NAT rule and explicit firewall block rules. Either one
alone is sufficient; both together mean a misconfiguration in one layer
does not break isolation. Second, VLAN segmentation is enforced both by
firewall policy and by the VMware Host-only network architecture — I
validated in Phase 10 that disabling the pfSense firewall entirely still
prevented cross-VLAN communication because the virtual switches are
physically separate.

---

Q: How did Suricata detect SQLMap specifically?

A: Two ways simultaneously. The Emerging Threats ET SQL rules matched on
SQL syntax patterns appearing in the HTTP GET parameter — things like
UNION SELECT, OR 1=1, and information_schema references URL-encoded in
the id parameter. At the same time my custom rule SID 9000002 matched on
SQLMap's default user-agent string, which it sends with every request
unless explicitly changed. The tool was flagged before it even completed
its first injection test. In a real environment an attacker would change
the user-agent, which is why the payload-based ET rules provide the more
robust detection layer.

---

Q: What would you add to this lab if you had more time?

A: Three things. First a SIEM — forwarding Suricata EVE JSON to Security
Onion or a Splunk free instance would enable timeline correlation across
both monitored interfaces and dashboard visualisation of the attack
session. Second I would re-run all attacks against DVWA Medium and High
security levels to show how input validation and prepared statements defeat
the same SQL injection and XSS attacks that succeeded at Low — that
demonstrates the application layer defence side. Third I would add a
honeypot on the Management VLAN at a static IP with no legitimate service
running — any connection attempt to that IP is definitionally malicious
and would generate a high-confidence alert for lateral movement.

---

Q: How does this lab relate to real enterprise environments?

A: The architecture maps directly to production. pfSense is functionally
equivalent to a Cisco ASA, Palo Alto NGFW, or Fortinet FortiGate — the
concepts of zones, interface rules, NAT policy, and stateful inspection
are identical. Suricata is deployed in many production SOCs alongside or
instead of commercial IDS products, and the Emerging Threats ruleset is
used in real environments. The VLAN segmentation model — Management,
Users, DMZ, guest or isolated networks — is standard in every enterprise
above 50 users. The attack sequence follows the PTES penetration testing
methodology and the tools are identical to what a real assessor would use.
The main difference is scale — a production environment would have
redundant firewalls, a SIEM, and many more hosts per VLAN.
