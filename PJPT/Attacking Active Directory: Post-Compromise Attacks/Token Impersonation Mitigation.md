## Token Impersonation Mitigation

### Limit user/group token creation permission
- Restrict which users and groups can create or manipulate tokens to prevent abuse
- Helps block attackers from creating or modifying tokens for privilege escalation

### Account tiering (Privileged Access Model)
- Best practice for securing privileged accounts
- Example:
  - If Bob is a Domain Admin, he should only log into the Domain Controller (DC) with his DA account
  - If Bob logs into a workstation with a lower-privilege account, even if that machine is compromised, his DA tokens won’t be exposed
- Avoid using high-privilege accounts for day-to-day tasks—use separate accounts for admin and regular activities

### Local admin restriction
- Limit the number of users who have local admin rights to reduce token theft risks
- Prevents attackers from accessing and stealing tokens on compromised machines
