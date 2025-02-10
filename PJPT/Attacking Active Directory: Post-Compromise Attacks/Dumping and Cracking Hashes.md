## Dumping and Cracking Hashes

### Secrets Dump with password
- `secretsdump.py <domain>/<username>:'<password>'@<ip>`

  ![image](https://github.com/user-attachments/assets/89c1e61e-63a6-4512-bf63-81939609e2fb)

  Things to note:
  - SAM hashes (admin and users)
  - DCC2 hash (to try to crack)
  - Cleartext passwords (if any)
    
- Dump secrets for all machines you have logon

  ![image](https://github.com/user-attachments/assets/c936e6a1-c058-42ca-8edc-2d5bc0f74bd7)

- Consider scripting for all machines

### Secrets Dump with Hash
- `secretsdump.py <user>@<ip> -hashes <hash>`

  ![image](https://github.com/user-attachments/assets/3ed684fa-f323-4e92-9886-9561b43b5264)

- Keep spraying network with new accounts found

### Cracking Hash
- Crack the LM: `hashcat -m 1000 lm.txt /usr/share/wordlists/rockyou.txt`

  ![image](https://github.com/user-attachments/assets/04fd158e-c9f1-4159-8e9a-d02b3f2afc76)

- So far, not likely to be picked up by antivirus
