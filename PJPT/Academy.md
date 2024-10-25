## Machine Information
- **Name**: Academy
- **Difficulty**: Easy
- **OS**: Debian
  
## Tools Used
- nmap: For port scanning and service enumeration.
- hashcat: To crack password hashes.
- ffuf: To fuzz for directories and subdomains.
- linpeas: For local privilege escalation (PE) checks.
- pspy: To monitor processes for privilege escalation.

## Objectives
- Enumerate the system and identify services.
- Discover vulnerabilities within the system.
- Exploit the vulnerabilities to gain initial access and user access.
- Escalate privileges to root.
  
## Enumeration
1. Nmap scan
  
    - The first step was to enumerate open ports and services using nmap:
    
        `nmap -A -vv <target IP>`
    
    - Open Ports:
      
      - 21/tcp (FTP) - Anonymous login allowed.
      
      - 22/tcp (SSH) - Open but no further details yet.
      
      - 80/tcp (HTTP) - Web server hosting a website.
  
2. FTP Enumeration

    - Connected to the FTP server using anonymous login:
    
        `ftp <target IP>`
    
    - Discovered a file called note.txt, which I downloaded and read:
      
        `get note.txt`
      
        `cat note.txt`
    
    - Note info
      
      - The note mentioned that "Grimmie" set up a test website for a new academy.
      
      - It included a command for adding a user into the database, with a registration number and a password hash.
      
      - The note was signed by "jdelta."

3. Port 80 HTTP Enumeration:
  
    - Ran ffuf for directory fuzzing:
      
        `ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ - u http://<target IP>/FUZZ`

    - Discovered two subdomains: /academy (a test website) and /phpmyadmin (MySQL admin interface).
     
## Vulnerability Discovery
1. Hash Cracking
  
    - Used hash-identifier to identify the hash type as MD5.
  
    - Cracked the hash using hashcat and the rockyou wordlist:
  
      `hashcat -m 0 hash.txt /usr/share/wordlists/rockyou.txt`
  
    - Cracked password: student.

2. Login to Academy Website:

    - Navigated to the /academy website.
  
    - Used the registration number and the cracked password (student) to log in.
  
    - Explored the web app and found a file upload feature on the /my-profile.php page, where users could upload profile pictures.

## Exploitation
1. Initial access:

    - Instead of uploading a normal image, I prepared a PHP reverse shell payload.
  
    - Set up a listener on my machine with netcat:
  
      `nc -lvnp 5555`
  
    - Uploaded the malicious PHP file and triggered the file to execute, resulting in a reverse shell as the www-data user.

2. User access:
  
    - Ran linpeas to gather system information. Discovered:
  
      - Backup script associated with the user "grimmie."
     
      - Hardcoded MySQL credentials in a PHP configuration file.
     
    - Used the discovered credentials to SSH into the machine as the user grimmie.

## Privilege Escalation
- Ran pspy to monitor running processes.

- Discovered that the backup script executed periodically and likely ran as root.

- Replaced the contents of the backup script with a bash reverse shell payload to gain root access.

- After modifying the backup script, I set up another netcat listener:

  `nc -lvnp 4444`

- When the backup script executed, it triggered the reverse shell and granted me root access.

## Post-Exploitation
- Navigated to /root and retrieved the flag.

## Lessons Learned
- While the machine had some CTF-like elements, it still provided important insights into real-world scenarios, such as the use of insecure backup scripts and poor credential management.
