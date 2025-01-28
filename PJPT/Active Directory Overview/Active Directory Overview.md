## Active Directory Overview
- Part of internal penetration testing
- What if we're tasked with an internal pen test?
- What happens if we compromise and get inside the network?
- What happens if we do a physical pen test and drop a machine off?
- Most likely, we'll be up against active directory

### What is Active Directory?
- Very much like a phone book for Windows
- Directory service developed by Microsoft to manage Windows domain networks
- Stores information related to objects (computers, users, printers, etc.)
- Authenticates using Kerberos tickets
- Non-Windows devices can authenticate to AD via RADIUS or LDAP
- Most commonly used identity management service in the world
- Can be exploited without ever attacking patchable exploits by abusing features, trusts, components, etc.
- "It's a feature, not a bug"
