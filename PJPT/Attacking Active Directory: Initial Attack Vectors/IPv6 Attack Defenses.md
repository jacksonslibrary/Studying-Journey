## IPv6 Attack Defenses
- Ideally, IPv6 should be disabled, but this can cause unintended consequences
- IPv6 poisoning abuses the fact that Windows queries for an IPv6 address even in IPv4-only environments. If you do not use IPv6 internally, the safest way to prevent mitm6 is to lock DHCPv6 traffic and incoming router advertisements in Windows Firewall via Group Policy. Disabling IPv6 entirely may have unwanted side effects. Setting the following predefined rules to Block instead of Allow prevents the attack from working:
  - (Inbound) Core Networking - Dynamic Host Configuration Protocol for IPv6 (DHCPv6-In)
  - (Inbound) Core Networking - Router Advertisements (ICMPv6-In)
  - (Outbound) Core Networking - Dynamic Host Configuration Protocol for IPv6 (DHCPv6-Out)
- If WPAD is not in use internally, disable it via Group Policy and by disabling the Win HTTPAutoProxySvc service
- Relaying LDAP and LDAPS can only be mitigated by enabling both LDAP signing and LDAP channel binding
- Consider Administrative users to the Protective Users group or marking them as Account is sensitive and cannot be delegated, which will prevent any impersonation of that user via delegation
  
