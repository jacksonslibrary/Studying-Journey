# 5th Capstone Machine
This is my write-up for the fifth machine in the capstone section of the Practical Ethical Hacking course.

## Machine Information
- **Name**: Blackpearl
- **Difficulty**: 
- **OS**: Debian
  
## Tools Used
- nmap: For port scanning and service enumeration.

## Objectives
- Enumerate the system and identify services.
- Discover vulnerabilities within the system.
- Exploit the vulnerabilities to gain initial and user access.
- Escalate privileges to root.
  
## Enumeration
1. Nmap scan:
  
    - The first step was to enumerate open ports and services using nmap:
    
        `nmap -A -v -p- 10.0.2.154`

  ![image](https://github.com/user-attachments/assets/e6d222a0-b2ae-4a85-a59f-0214576f0396)

  
2. Port 80 HTTP Enumeration:

    - nginx install page.
  
    - Fuzzing for directories did not result in futher findngs.
  
3. DNS Enumeration:

    - Used dnsrecon to scan for dns records.
  
        `dnsrecon -r 127.0.0.0/24 -n 10.0.2.154 -d domain`

  ![image](https://github.com/user-attachments/assets/6d400e76-4f4d-4a8b-a710-49a5bf0464cf)

    - Added the record to /etc/hosts.

    - http://blackpearl.tcm showed a PHP info page.

    - Ran ffuf for directory fuzzing:

        `ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ -u http://blackpearl.tcm/FUZZ`

    - Found the directory /navigate which showed a login page for Navigate CMS
     
## Vulnerability Discovery
- Researched for Navigate CMS vulnerabilities and found an unauthenticated RCE vulnerability that has a metasploit module.
    
## Exploitation
  - Using the metasploit module, I set the rhost at blackpearl.tcm.

  - Ran the exploit and was dropped into a meterpreter shell.
