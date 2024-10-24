## Machine Information
- **Name**: Blue
- **Difficulty**: Easy
- **OS**: Windows 7 Ultiamte
  
## Tools Used
- nmap
- Metasploit

## Objectives
- Scan machine for information
- Identify vulnerability
- Exploit vulnerability

## Enumeration
I ran an nmap scan to check for open ports

`nmap -A -vv <IP>`

This told me port 135 is open running Windows RPC, port 139 is open running Windows netbios, and port 445 is open running SMB

## Vulnerability Discovery
I looked up vulnerabilities for Windows 7 7601 and found a vulnerability called EternalBlue.

I searched for EternalBlue on Exploit Database and found there was already an exploit for it.

Running Metasploit, I searched for eternalblue and found an auxiliary for EternalBlue.

Using the auxiliary, I looked at the options and then set the RHOST to the target machine address and ran it.

The auxiliary told me the machine is likely vulnerable to MS17-010.

## Exploitation
Metasploit already has an exploit for EternalBlue, using it I set the RHOST to the target machine address.

I want to have a nice, stabilized shell, so I set the payload to `windows/x64/meterpreter/reverse_tcp.

After setting the LHOST to my IP address, I ran the exploit.

This gave me a reverse shell and by running the command whoami, I knew I had system privileges.

## Lessons Learned
This may be a simple attack, it is still relavant and common on organization's internal networks.
