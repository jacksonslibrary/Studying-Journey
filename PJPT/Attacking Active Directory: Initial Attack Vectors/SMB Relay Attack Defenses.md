## SMB Relay Attack Defenses 

### Enable SMB Signing on all devices
- Pro: Completely stops the attack
- Con: Can cause performance issues with file copies (10 - 20%)

### Disable NTLM authentication on network
- Pro: Completely stops the attack
- Con: If Kerberos stops working, Windows defaults back to NTLM

### Account tiering:
- Pro: Limits domains admins to specific tasks (e.g. only long onto server with need for DA)
- Con: Enforcing policy may be difficult
- Should be done regardless

### Local admin restriction
- Pro: Can prevent a lot of lateral movement
- Con: Potential increase in the amount of service desk tickets
- Should be done regardless
  
