# DVWA Deployment Configuration

## Server Details
- OS:           Ubuntu Server 22.04 LTS
- Hostname:     dvwa-server
- IP Address:   10.30.30.10/24 (static)
- Gateway:      10.30.30.1 (pfSense DMZ interface)
- VLAN:         30 (DMZ)
- VMnet:        VMnet4

## Services
- Apache2:      running, enabled on boot
- MariaDB:      running, enabled on boot
- PHP:          8.x with mysqli, gd, xml, mbstring

## DVWA Config
- URL:          http://10.30.30.10
- Login:        admin / password
- Security:     Low (for attack simulation)
- DB name:      dvwa
- DB user:      dvwauser
- DB host:      127.0.0.1

## Credentials (lab only — never use in production)
- OS user:      labadmin / [REDACTED]
- MariaDB root: [REDACTED]
- DVWA DB user: dvwapass@2024!
- DVWA app:     admin / password