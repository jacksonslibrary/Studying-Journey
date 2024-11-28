# #1 Broken Access Control and #2 Cryptographic Failures

## Introduction
- This course the first 2 categories in the 2021 OWASP Top 10
  - Broken Access Control
  - Cryptographic Failures
- Describe each one
- Provide real world examples
- How to prevent them

## Broken Access Control

### What is broken access control?
- Unspecified permissions
- Improperly enforced permissions
- Access beyond a specific role
- Examples:
  - End user has the same access as an admin account
  - Accessing someone's account
- Impacts:
  - Unintended change to the integrity of an asset
  - Failure to protect confidentiality of information
  - Breach or theft of a stolen asset
- If someone has access to information that their role is not supposed to, they can manipulate, change, or steal that data

### Example 1: 2021 Facebook broken access control vulnerability
- Laxman Muthiyah reported a broken access control vulnerability to Facebook in 2021
- Vulnerability was found in Facebook Business Manager
- FBM allows business to organize and manage a company account on Facebook
- FBM allows adding roles such as employee, admin, finance analyst etc.
- Vulnerability allowed 3rd-party app to add a new admin account on a Facebook account page with limited permissions
- Victim would permanently lose admin access to the page
- Attacker could remove legitimate admins from FBM

### Example 2: 2021 personal data travel breach
- Bob Diachenko discovered a database containing plaintext PII of over 100 million travelers to Thailand in 2021
- Due do lack of proper access controls, the database was publicly facing
- Reported to Thai Authorities and incident was resolved in a matter of days

### Prevention techniques: Least privilege
- Application can create strong access controls
- Restricts access to the minimum required to do work
- Access is denied by default
- Access starts at 0
- Access is added granularly for what is needed
- Web app devs should apply least privilege for human and service accounts
- A secret manager can apply least privilege to secrets
- To implement least privilege; allow access based on the most granular control possible

### Prevention techniques: Record ownership and logging
- Record: single unit of data that should be classified and protected, fields associated with an object
- Record access types: read, write, modify, and delete
- User must be authenticated to the app and authorized to the record
- Also known as personal access control, based on record, not field
- Logging compliments record ownership
- Logging keeps track of certain activities including user, record, and access level
- Monitoring access control logs can detect incidents and find patterns

### Prevention techniques: Functional access control testing
- Access control needs to be tested to determine if it aligns with policy
- Expected and unexpected paths should be tested
- Tests should attempt to misuse and abuse the software
- OWASP provides a web testing guide, including an authorization section
- Tester should verify:
  - Can sensitive data be accessed if a user is not authenticated?
  - Can sensitive data be accessed after a user has logged out?
  - Can sensitive data that should only be accessed by privilege roles be accessed by a nonprivileged user?
  - Can admin functions be accessed if the tester is logged in as a nonadministrative user?
  - Who should not be able to access admin functions?
 
## Cryptographic Failure

### What is cryptographic failure?
- When cryptography doesn't work as intended
- Cryptography protects messages from being read
- Modern encryption algorithms are strong and protect data well
- Confidential data can be accessed if not encrypted or encrypted poorly

### Example 1: 2021 GoDaddy plaintext passwords
- GoDaddy reported a data breach that included plaintext passwords in 2021
- SFTP authenticated used plaintext passwords
- Active usernames and passwords were accessible for about a month and a half
- Allowed attacker to authenticate to SFTP and access files, change configurations, or change file permissions for WordPress sites
- Flaw was fixed immediately 

### Example 2: Using a broken or risky cryptographic algorithm
- NVD reports about 9 cases of broken or risky cryptographic algorithm between 2017 and 2021
- Vulnerabilities were found in a mail server, medical equipment software, email fowarding service, electric equipment, 2 kinds of security software, and video conference software
- Likely used an out of date cryptographic algorithm
- Strong algorithms can still be broken
- Cryptographic algorithms no longer provide protection as new attacks and computinig power adavnce
