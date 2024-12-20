# Specialized Testing: XSS
- This is not part of the Practice Ethical Hacker Course, but I found it relevant to preparing for the PJPT
## Course Overview
- XSS is an attack known since the late 90s
- There are many different types
- Web apps need to be prepared to defend against them

Plan:
- Different types of XSS
- Reflected, stored, and DOM-based XSS
- Test a web app for XSS
- Look for XSS patterns

Prerequisites:
- HTTP
- HTML
- JavaScript

## XSS Fundamentals
### History of XSS
- Microsoft security engineers coined the term in Jan 2000
- XSS worm; vulnerability in Hotmail found in October 2002 by Berend-Jan Wever
- Samy (MySpace worm) Myspace exploit by Samy Kamkar in Ocotober 2005
- Old attack but still there
- Doesn't go away, more and more relevant

### Same-Origin Policy
- Auditor job: usually is to find a vulnerability, prove there is one, and there is an exploit
- Same-origin policy is supposed to protect apps ("soul security JS mechanism")
- Same-origin policy says JS only has access to things of same domain, protocol, and port

Example:
- Web page uses JS 3 times; 1 inline block and 2 external JS files
- One is a script file that resides in the same origin
- The other is pulled from a CDN, a JS file on another server
- The file from the CDN is also the same origin otherwise jquery wouldn't be able to access any elements on the page
- The origin of any JavaScript code is defined by the origin of the page that executes it
- Origin is not literal. JS runs in the security context of the page
- If you can inject JS, it runs with the same privileges and security context of the page

### Executing JavaScript Code





