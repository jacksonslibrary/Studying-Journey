# OWASP Top 10 Notes: Learning the OWASP Top 10

## Introduction

### A first look at the OWASP Top Ten

- OWASP: Open Web Application Security Project
  - Provides resources for learning security best practices
  - Publish a list of the top 10 most common AppSec vulnerabilities every few years

### Broken Access Control

- "This exposure occurs when confidential information is viewed by a user who should not be able to access that data." - OWASP
- Access Control: Access for people that are allowed and not for those who aren't
- Broken access control breaches CIA of a web app
- Unintended access could read, write, or edit sensitive data
- Occurs when user acts beyond the permissions of their role
- Don't leave private data in public
- Make sure you send data to the right person
- Positive path is most commonly test while alternatives are left out
- Need to assume misuse will occur
- Building solid access control into a web app is critical

### Cryptographic Failure
- "The first thing is to determine the protection needs data in transit and at rest." - OWASP
- 2 parts: Sensitive data should be protected with encryption, encrypted data should have effective encryption
- Don't unnecessarily store sensitive data
- Encrypt sensitive data at rest and in transit
- Use known strong encryption algorithms

### Injection
- Code can represent data or instruction
- Injection happens when an app accepts data an input and processes it as an instruction
- "An application is vulnerable to attack when hostile data is used." - OWASP
- Incorrect handling of input can allow injection
- Unintended outcomes can be caused by malicous input
- Cross-site scripting is a version that doesn't neutralize user input
  - Doesn't check the input is as intended
- SQL injection is injection attacking a SQL database
- User input must be neutralized or verified

### Insecure Design
- "A new category focusing on risks related to design and architectural flaws." - OWASP
- Addresses design-level flaw-type vulnerabilities rather than code-level vulnerabilies
- Design errors can be the root cause of much larger problems
- Examples:
  - Sensitive information in error message
  - Passwords stored in plaintext
- Security matters in the design phase, not just when writing code
- These decision need to be made early in the SDLC

### Security Misconfiguration
- "The application might be vulnerable if it is without a concerted, repeatable, application security process." - OWASP
- Something that is security configured is considered hardened
- Example: not using a password on mobile device
- Examples: 
  - Not using a passcode on a mobile device
  - Failing to change default passwords
  - Failing to use strong passwords
  - Enabling unnecessary services or features
  - Improper cloud permission services
  - Failing to update software
- Each setting and configuration should be indentified and assessed for security
- Follow Center for Internet Security (CIS) guidlines and standard
- Address risk tolerance and requirements specific to organization
- All teams should understand to make secure decisions
