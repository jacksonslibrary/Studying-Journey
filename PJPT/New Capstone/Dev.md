# 3rd Capstone Machine
This is my write-up for the third machine in the capstone section of the Practical Ethical Hacking course.

## Machine Information
- **Name**: Dev
- **Difficulty**: Easy
- **OS**: Debian
  
## Tools Used
- nmap: For port scanning and service enumeration.
- ffuf: To fuzz for directories and subdomains.
- showmount and mount: To enumerate and mount NFS shares.
- zip2john: To convert zip file to hash.
- john: To crack password hash.

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
      
      - 22/tcp (SSH) - Will skip for enumeration
      
      - 80/tcp (HTTP) - Apache 2.4.38 server, Bolt installation error
     
      - 2049 (NFS) - Could mount for enumeration
     
      - 8080 (HTTP) - Apache 2.4.38 server, PHP 7.3.27
     
      - Other ports reviewed and found non-critical (rpcbind, mountd, nlockmgr)
  
2. Port 80 HTTP Enumeration:

    - Connected to web server on port 80.
  
    - Bolt installation page.
  
    - Looks like it's running Bolt CMS.
  
    - Current folder: /var/www/html/.
  
    - Ran ffuf for directory fuzzing:
      
        `ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ - u http://<target IP>/FUZZ`

    - Discovered subdomain /app, a file directory
  
    - Contained a config.yml file containing database credentials
    
3. Port 8080 HTTP Enumeration:
     
    - PHP info page, some information disclosure.
  
    - Ran ffuf for directory fuzzing:
      
        `ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ - u http://<target IP>:8080/FUZZ`

    - Discovered subdomain /dev, a BoltWire app
  
4. Port 2049 NFS Emuneration:

    - Check the mounted file share for a directory:
      
        `showmount -e <target ip>`

    - File share /srv/nfs offered up.

    - Mounted share:

        `mkdir /mount/directory`
      
        `sudo mount -t nfs <target ip>:/target/directory /mount/directory`
      
     - Contained a password protected zip
  
     - Extracted and cracked it using zip2john and john
  
         `zip2john save.zip > hash.txt`

         `john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`

     - This gave me the password to the zip file, which contained an id_rsa file and a todo note from jp.
     
     
## Vulnerability Discovery
1. Cracking zip:
   
     - Bruteforced with zip2john and then john
  
         `zip2john save.zip > hash.txt`

         `john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`

     - This gave me the password to the zip file, which contained an id_rsa file and a todo note from jp.
    
2. BoltWire App - LFI Vulnerability:

    - An Exploit Database shows that BoltWire has a Local File Inclusion vulnerability that allows authenticated users to read sensitive files on the server via path traversal.
  
    - I registered a user on the app.
  
    - I changed the action paramter in the URL to traverse to /etc/password.
  
         `http://<target IP>/boltwire/index.php?p=action.search&action=../../../../../../../etc/passwd`

   - This displayed the /etc/password file.
  
   - One user's name is jeanpaul, matching the initials to the note.

## Exploitation
  - Using the username from /etc/password, id_rsa, and the database password as the keyphrase for id_rsa, I logged via ssh as jeanpaul
  
## Privilege Escalation
   - Checked which commands user can run as sudo and found zip can be run as sudo without password

   - Ran zip the zip as root using commands from GTFOBins

       `TF=$(mktemp -u)`
     
       `sudo zip $TF /etc/hosts -T -TT 'sh #'`

   - This dropped me into a shell as root

## Post-Exploitation
- Navigated to /root and retrieved the flag.

## Lessons Learned
- This machine emphasized the value of thorough enumeration, which led to discovering a key misconfigured NFS share, and reinforced my skills in tools like ffuf for web fuzzing and mount for network file sharing. Exploiting a Local File Inclusion vulnerability in BoltWire highlighted the risk of path traversal flaws, while using GTFOBins for privilege escalation with zip demonstrated the dangers of granting sudo access to common binaries. Overall, this machine emphasized the importance of careful configuration and restricted access to sensitive directories in real-world security settings.
