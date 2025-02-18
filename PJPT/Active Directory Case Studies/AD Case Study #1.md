## AD Case Study #1

### You Spend How Much on Security?
- Internal Pentest
- Against a large US hospital
- Spent millions on security measures
- Defenses: IPv6 disabled, IDS/IPS, PAM, EDR, patch management
- Nessus showed nothing to directly exploit
- What is the attack strategy?

### Attack
- Set up relay attack
- SMB signing disabled on a machine
- Passed the hash
- Never compromised a domain account
- Dumped SAM
- Cracked password used to disabled EDR
- Password reuse is prominent
- Moving laterally now
- Just need to find 1 mistake
- Found a hash on one of the machines and could logon to DC as system

### Lessons Learned
- Enable SMB Signing and disable LLMNR
- Least privilege
- Account tiering
- Stop password reuse
