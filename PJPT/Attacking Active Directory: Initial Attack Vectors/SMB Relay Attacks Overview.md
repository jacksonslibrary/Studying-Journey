## SMB Relay Attacks Overview
Instead of cracking hashes gathered with Responder, we can instead relay those hashes to specific machines and potentially gain access

### Requirements
- SMB signing must be disabled or not enforced on the target
- Relayed user credentials must be admin on machine for any real value

### Identifying Hosts
- Identify hosts without SMB signing using nmap
- Use the smb2 security mode script: `namp --script=smb2-security-mode.nse -p445 <subnet>`

### Responder
- Turn off SMB and HTTP
- Make sure the hashes get relayed
- Run responder: `sudo responder -I eth0 -dwP`

### NTLM relay
- Set it up with a target file, hosts identified with SMB signing disabled or not enforced
- When responder captures a hash, it will be forwarded to NTLM relay, which forwards it to the targets
- If we are a local admin, we get some wins
- Run NTLM relay: `ntlmrelayx.py targets.txt -smb2support -i`
- `-i` is for an interactive shell

### Required Event
- Victim points to attacker

### Win
- Relay processes
- SAM hashes are dumped
- Shell opens
- Create local admin for persistence
