## IPv6 Attacks Overview
- DNS takeover attacks via IPv6
- Another form of relaying but more reliable

### Overview
- Machine usually runs on IPv4 but has IPv6 turned on
- If we're using v4 but v6 is turned on, who is doing DNS for v6? Usually nobody
- Attacker can spoof the IPv6 DNS server collecting all IPv6 traffic
- Can get authentication to DC via LDAP or SMB
- Wait for someone to use credentials, NTLM comes to us
- LDAP relay NTLM to DC, if DA, then login to DC, and create and account using mitm6
