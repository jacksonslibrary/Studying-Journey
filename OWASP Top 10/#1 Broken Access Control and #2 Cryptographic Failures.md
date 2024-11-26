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
  - Uninted change to the integrity of an asset
  - Failure to protect confidentiality of information
  - Breach or theft of a stolen asset
- If someone has access to information that their role is not supposed to, they can minipulate, change, or steal that data

### Example 1: 2021 Facebook broken access control vulnerability
- Laxman Muthiyah reported a broken access control vulnerability to Facebook in 2021
- Vulnerability was found in Facebook Business Manager
- FBM allows business to organize and manage a company account on Facebook
- FBM allows adding roles such as employee, admin, finance analyst etc.
- Vulnerability allowed 3rd-party app to add a new admin account on a Facebook account page with limited permissions
- Vicitim would permanently lose admin access to the page
- Attacker could remove legitimate admins from FBM
