## Domain Enumeration with Bloodhound
- Very famous and popular
- Install the latest version: `sudo apt install -y bloodhound`

### neo4j
- Required to run bloodhood
- Run it: `sudo neo4j console`
- Change password if necessary

### bloodhound
- In a bloodhound directory: `sudo bloodhound-python -d <domain> -u <username> -p <password> -ns <DC IP> -c all`

  ![image](https://github.com/user-attachments/assets/92c4fc58-aafa-4c45-831a-87d41cc50ea3)

- Open bloodhound and upload all the data
- Look through Analysis
- Select node and look at Node Info
- There is so much going on
- Mark nodes as desired (e.g. Owned)
- Create custom queries
- Etc.

  ![image](https://github.com/user-attachments/assets/db45523e-530e-4d16-b403-89a545504814)
