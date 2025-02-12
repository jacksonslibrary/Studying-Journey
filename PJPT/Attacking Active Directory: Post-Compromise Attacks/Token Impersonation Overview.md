## Token Impersonation Overview
- What are tokens?
  - Like cookies for computers
  - Temporary keys that allow you access to a system/network without having to provide credentials each time you access a file
- Relevant types of tokens
  - Delegate - Created for logging into a machine or using Remote Desktop
  - Impersonate - "non-interactive" such as attacking a network drive or a domain logon script

### Domain User Impersonation
- Can be done within Metasploit through the Incognito module
- Lists out available tokens
- Look for users with delegate token
- Impersonate user and jump into shell as user
- Attempt to dump hashes as non-Domain Admin

### Domain Admin Impersonation
- What happens if we compromise a domain admin?
- Domain Admin token is available
- Impersonate Domain Admin and jump into shell as administrator
- Attempt to dump hashes as Domain Admin
- Can run Mimikatz
- Dump hashes from domain controller
- Win!

### Create a New Domain Admin
- Impersonate a Domain Admin account
- Create a new user and then add it to Domain Admins group
- Run secrets dump against the domain controller and compromise it
- No need to compromise Domain Admin password
- Only need Domain Admin to logon
- Essentially become a Domain Admin
