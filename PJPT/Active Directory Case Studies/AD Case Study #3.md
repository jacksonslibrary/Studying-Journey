## AD Case Study #3
- No users were local admins
- LAPS (Local Administrator Password Solution) made every machine have a unique admin passwords
- Difficult to time MITM6 when domain admin logs on

### Path
- Hashes flying
- Crack weak passwords
-  One account had access to a file share
-  Could find workstation setup procedure
-  Macbook Pro Setup Procedure has admin password
-  Creds worked on one machine
-  Dump secrets and find a service deployed and clear text password
-  Service account was a domain admin

### Lessons Learned
- This level of enumeration separates the average from the great
- Dig deep to find the answer
