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
- We  must know how JS code may be executed to exploit the vulnerabilities
- There are 2 main approaches

\<Script> element:
- Reference an external file
- Local file
- Different server
- Inline JS
- Inline is commmon for testing:
```
<script>
  doStudd();
</script>
```

HTML attribute:
- Start with on and then the name of an event:
```
<input
  oninput="doStuff()"
  type="text">
```
- The code will run when the even is fired
- Over 100 different events
- Some web apps use a denial list of events

Mutated XSS:
`<img ="><script>alert(1)</script>">`
- It looks like an image tag and an attribute without a name
- Value in double quotes with angle brackets
- HTML doesn't have to be 100% correct
- Browsers compensate, too much here
- Replaces attribute value with JS code
- Specific vector used to evade some filters

### Types of Cross-Site Scripting
Reflected XSS:
- Type 1
- Attack is part of the HTTP request
- Request contains malicous JS code etc.
- Reflected means returned by the server
- Most common

Stored XSS:
- Type 2
- Permanent
- JS code injected into app affects other users
- More dangerous than reflected XSS
- Less common

DOM-based XSS:
- Type 0
- Everything happens in the browser
- No server involved
- Super rare but increasing with SPAs

## Reflected XSS
### Introduction:
- Find and exploit in demo app
- Demo app is for events and ticktts
- Has login and search features

### Reflected XSS in Detail
- Often typical patterns

Example:
- A search function that displays the search term on the results page
- In PHP: "You searched for: `<?= $_GET['q'] ?>`"
- This is insecure processing of the search query
- The search query can be a payload: `<script>alert(1)</script>` resulting in reflected XSS
- In an HTML attribute: `<input type="text" name="q" value="<?= $_GET['q'] ?>`
- Also insecure processing of the search query but requires triggering an event
- `<script>alert(1)</script>` would be treated as text
- Set up JS event handler: `" onmouseover="alert(1)` executing the JS when the mouse moves over the text

### Demo: Finding Reflected XSS








