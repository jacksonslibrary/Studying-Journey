## Passback Attacks
- We're looking for access to something that connects to LDAP, an SMB connection, etc.

### Printer's LDAP Setup
- Usually has an IP address, username, and a redacted password
- The IP address likely points to the DC or LDAP server
- Change this to the attacker IP address and setup a listener
- When a user authenticates to use the printer, their password is sent in cleartext
- Don't forget about this, could be huge
  
https://www.mindpointgroup.com/blog/how-to-hack-through-a-pass-back-attack/
