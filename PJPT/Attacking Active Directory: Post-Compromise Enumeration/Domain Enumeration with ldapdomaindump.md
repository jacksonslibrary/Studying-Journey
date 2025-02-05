##  Domain Enumeration with ldapdomaindump
- `sudo /usr/bin/ldapdomaindump ldaps://<DC IP> -u '<domain>\<username>' -p <password>`

  ![image](https://github.com/user-attachments/assets/914b96c6-d3c2-4981-86cc-3214751b16bd)

- `firefox domain_users_by_group.html`
- So much information
- Things to note:
  - High value targets: Domain Admins, Enterprise Admins
  - Domain Users: More access
  - What else is available?
