# Studying Journey
Welcome to my journey in studying penetration testing, where I document what I'm learning through machine write-ups. This repository serves as a personal log and resource as I prepare for various security certifications and challenges as well as for the joy of learning.

## Current Focus
I am currently preparing for the **Practical Junior Penetration Tester (PJPT)** certification from TCM Security. This repo will include:
- Machine write-ups for rooted machines.
- Tools and methodologies used.
- Lessons learned from each challenge.

## Table of Contents
1. [About the PJPT Exam](#about-the-pjpt-exam)
2. [Machine Write-Ups](#machine-write-ups)
3. [Tools & Techniques](#tools--techniques)
4. [Lessons Learned](#lessons-learned)

## About the PJPT Exam
The **Practical Junior Penetration Tester (PJPT)** exam focuses on assessing skills in:
- Enumeration
- Vulnerability discovery
- Exploitation
- Privilege escalation
- Reporting findings

This repository is part of my preparation to reinforce key concepts and gain hands-on experience by rooting various vulnerable machines.

## Machine Write-Ups
Below are the detailed write-ups for each machine I've completed. Each write-up includes enumeration, exploitation, and post-exploitation details.

- [Blue](PJPT/Blue.md)
- [Academy](PJPT/Academy.md)
- [Dev](PJPT/Dev.md)
- [Butler](PJPT/Butler.md)
- [Blackpearl](PJPT/Blackpearl.md)

## Tools & Techniques
Throughout this process, I've used a variety of tools and techniques for different phases of penetration testing:

- **Nmap**: Network discovery and port scanning.
- **netcat**: To create port listeners.
- **Hashcat**: Password cracking.
- **ffuf**: Directory and subdomain fuzzing.
- **LinPEAS**: Linux privilege escalation enumeration script.
- **Metasploit**: Exploitation framework.
- **pspy**: Tool for monitoring processes for privilege escalation.
- **john**: To crack password hashes.
- **winPEAS**: To search for possible local privilege escalation paths on Windows.
- **MSFvenom**: To create payloads.
- **Burp Suite**: For modifying requests and brute forcing logins.

## Lessons Learned
Throughout my studying journey, I've learned the following key lessons:
- The importance of thorough enumeration.
- How privilege escalation opportunities are often hidden in plain sight (e.g., weak credentials, misconfigured services).
- The value of automation balanced with manual exploration.
- The significance of clear, structured reporting.

## Contributing or Feedback
If you have any feedback, suggestions, or resources that might help me on this journey, feel free to reach out via creating an issue.
