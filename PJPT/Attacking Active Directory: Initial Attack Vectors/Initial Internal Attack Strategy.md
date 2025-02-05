## Initial Internal Attack Strategy

### Begin day
- Run mitm6 or Responder
- Get feet wet with Responder
- mitm6 can be too much of a cheat code
- Don't take down the network with mitm6

### Scans
- When running Responder, also run scans to generate traffic
- These are Nessus scans, vulnerability scans, etc.
- Responder can be run all day

### Websites
- If no dice so far, look for websites in scope
- Use `http_version` from Metasploit to sweep subnets
- Always be multitasking

### Default credentials
- Look for default credentials on web logins
- Check for Printers, Jenkins, etc.
- Could lead to DA

### Think outside the box
- Initial attacks wont always work
- Look at what else is available on the network
- If a few days have past with no dice, ask for credentials
- Enumerate with credentials
- DIG! DIG! DIG!
