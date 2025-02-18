## AD Case Study #2

### Digging Deep
- Internal Pentest
- Against a large US hospital
- Invested millions in security measures
- Defense: LLMNR and IPv6 disables, SMB Signing enables, IDS/IPS. A/V, patch management
- No entry level attacks

### Attack
- Sweep webpages
- Look for default credentials
- Tool in development had local admin password in plaintext
- Spray the password
- One machine compromised with password
- Dump secrets
- Hash matched other admin account
- Spray that credential
- Found old Windows with WDigest enabled and domain admin password in clear text
- Logon onto DC and compromise it

### Lessons Learned
- No default creds
- Turn off WDigest
- Don't give service accounts Domain Admin access
- No password reuse
