# Specialized Testing: Command Injection
- This is not part of the Practice Ethical Hacker Course, but I found it relevant to preparing for the PJPT
## Course Overview
- A critical web app vulnerability
- Allows attackers to attack infrastructure and steal information
- We'll look at different ways to test for it
- Major Topics:
  - Different types
  - Entry points
  - Testing a web app
  - Mitigations for it
- Prequisites:
  - Linux funamentals
  - HTTP Protocol
  - Penetration testing tools exposure

## Exploring Command Injection Attacks
### Introduction

Command Injection:
  - Vulnerability that allows an attacker to execute commands on an OS
  - Limited by the software on the system
  - Uses existing code or tools to execute commands in the context of a shell

Code Injection:
  - Vulnerability that allows an attacker to injects arbitrary code into a vulnerable app
  - Bound to the capabilities of the langauge
  - Hijacks an app to execute the code
  - Injection is ranked 3rd in frequency, severity and magnitude in OWASP Top Ten

CAPEC:
  - Common Attack Pattern Enumeration and Classification
  - List of attack patterns showing how related weaknesses can cause exploitation
  - Attack Pattern ID: 248
  - Maps to CWE and OWASP
 
### Shell Metacharacters
- Shell metacharacters: special characters or sequences of special characters
- Command injection depends on them to advance an attack
- `&`: allows you to execute multiple commands in succession on Windows
- `;`: same behavior used in Linux
- `|`: take the output from the left command and sends it as input to the right command
- `||`: OR operator, executes right command only if the left command fails
- `*`: a wildcard charcter
- `>` redirects the left command output to the right output stream, usually a file
- `\`: escape character, place before a metacharacter to use the literal value, or create special characters (`\n`,`\t`, etc)

Windows:
- `dir`: list files and directories of current location
- `whoami`: shows current account
- Combine them with `&` to execute both
- OR them with `||` to execute `whoami` if `dir` fails

Linux:
-`pwd`: show current directory
- `ls`: list files and directories of current location
- `whoami`: shows current account
- `id`: shows current account details
- Execute all of them by separating them with ` ; `

### Anatomy of an Attack
2 Types:
- Result-based: an attacker can see the output of the injected commands in the application response
- Blind Command Injection: The attacker cannot see the results for any inejected command

Components:
- Web (HTTP) Request: how we send information back to the web server
- Vulnerabilities are usually found in user supplied data that is used by the web app to execute server-side shell commands
- Injected Command: Use these opportunities to compromise the server
- Web (HTTP) Response: Attacker uses this information to exfiltrate data, pivot, escalate privileges, and many other actions

HTTP Request: GET
- Most common
- Attacker sends GET requests to see how server responds
- Modifies request to include a command to see if server reponds with executed command

HTTP Request: POST
- 2nd most common
- Usually a form submission or some API calls
- Attacker can include injected commands in input fields or intercept and manipulate the POST request

## Discovering Command Injection
### Demo: Code Review
Review code snippets:
- Python web app
- Content Management System (CMS)

Python web app:
- Line by line code review is time consuming
- Use grep instead: `grep -rE 'system|subprocess|exec|spawn' *`
- Open the file in vim
- Add line numbers: `:set number`
- Search for subprocess: `/subprocess`
- Code does not validate iput
- Command is passed directly to the OS shell with no safeguards

CMS:
- Use Psalm to automate finding security and semanti bugs in PHP code
- Dump the file with user input to the screen: `bat html/cool_tool.php`
- Psalm warns us unsafe shell_exec

### Demo: Finding Entry Points
Task: look for features or funcationality that we suspect to be invoking a system command on the back end

Inspection:
- Input fields
- HTTP request headers
- HTTP POST body
- URL parameters

Demo Website:
- Search box, usually for SQL injection
- Support page has a DNS and Diagnostics tools

DNS Tool:
- Executes a dig command based on user input, a potential entry point

Diagnostics tool:
- Executes a ping command based on user selection, a potential entry point
- Use browser dev tools to see request and reponse of clicking submit
- Uses a POST, entry point will be in the body of the request, not the url
- Headers are also a potential entry point, depending on how they are handled

Privacy page:
- Used in URL parameters
- Dumping the contents of the privacy.txt file to the page
- Another potential entry point

### Demo: Manual Testing
Tasks: 
- Test potential entry points to see if they're vulnerable
- Take actions from browser to command line for testing

Privacy page via browswer:
- Start by adding `;` to the end of the url
- Append a command `id`
- No extra output, may not be vulnerable or may be blind
- Encode the semicolon `%2b`
- This resulted in a blank page
- Trying with `cat%20etc/passwd` it dumps the passwd file
- Likely to be local file include since the `id` command didn't work
- Let's try a different metacharacter `&~ encoded as `%26`
- Appending `%26%26id` results in the privacy page with the id command executed
- Evidence to show successful command injection
- *If it did not work, that doesn't mean the application is not vulnerable. It only means that the tests performed did not reveal command injection.*

DNS tool via browswer:
- Try using semicolon again
- Submitting `fakelabs.org ; ls -l`  removed everything from the body of the page
- May not be vulnerable or it can be blind
- Let's pipe it to a file by appending ` >purple.txt`
- Browsing to the file resulted in a 404, not a blind attack
- Try command substitution with ` $(id)` which worked

Diagnostics tool via browswer:
- Let's intercept and modify the request before fowarding it
- Run Burp Suite and turn on interceptor
- The request includes the server address for the drop down menu's value (9.9.9.9 for Quad9)
- Append `;ls%20/bin` to inject command and foward request
- Browser shows server reponded with ping command and injected command
- Now replace the address with `||+ps+aux` and foward the request
- Browser shows proccesses running with limited visibility

Privacy page via CLI via browswer:
- Use `curl -k '<URL>'` to send requests
- Append `%3bid` encoding semicolon with `id` command to the URL
- This executes the command and shows it in the output

Diagnostics tool via CLI:
- Use `curl -k -x POST '<URL>' -d 'ip_addr=8.8.8.8&&id'`
- No command input
- Let's encode it with curl changing `-d` to `--data-urlencocode`
- The injection command executed


### Demo: Automated Testing
- Automation helps make our testing process sustianable and repeatable
- We'll look at using Burp Suite Pro and Commix

Automating with Burp Suite:
- In Burp Suite intercept submitting a Google connectivity check with the diagnostics tool
- Send the request using intruder with Sniper attack type
- Mark the payload location with markers
- Add the payload and url encode them
- Start the attack and focus on status code and length different from baseline
- The response with the longest length included the executed command

Automating with Commix:
- Short for command injection explorer
- Installed by default in Kali
- Pass it a parameterized URL: `commix -u '<URL>'`
- Commix identified the GET parameter 'policy' appears to be injectable via results-based command injection
- Can use a psuedo-terminal shell to enter commands
- Use a tamper list from `commix --list-tamper`: `commix -u '<URL>' -d 'ip_addr=1.1.1.1 --tamper=space2htab --technique=t`
- Commix identified the POST parameter 'ip_addr' appears to be injectable via bind time-based commang injection

## Mitigating Command Injection Attacks
### Input Sanitzation and Validation
- The root cause of command injection vulnerabilities in **unsanitzed user input**
- This allows the mixing of data and code

Input Sanitization:
- Modifies user input by removing unsafe characters
- A developer can use an allow list that filters the input
- Another option is to use a block list of common metacharacters that are removed from the input
- Or they can escape the data to neutralize the special meaning of certain characters

Input Validation:
- Ensures that data is received in a format that is suitable for the systems that need to process it
- Enforces semantic and structure rules

Process order:
- First validate the input
- If necessary, perform a final sanitization
- Sanitizing first can risk modifying input and cause inintended consequences
- User should be alerted of invalid input
- The context of the input can have varying requirements
  
### Principle of Least Privilege
- An entity is limited to only the resources and access needed to form its function
- Limits attacker capabilities once gained initial access

### Using Technical Controls
- Testing for command injections helps us understand the security posture of an app
- It should be conducted while observing the effectiveness of various technical controls

Technical Controls:
- Goal: address risk
- Audit logs: a record of events and activities of a network, system, or persona
- Access control list (ACL): a list of rules for controlling access to system or network resources
  
Web application firewall (WAF): 
- One of the best technical controls
- Monitors and filters web traffic to and from a protected web app, 
- Deployed as hardware, software, or cloud solution
- Great mitigation tool for command injection and many other web-based attacks
  
### Demo: Implementing Mitigations
Flawed block list:
- Single pipe character includes a space
- No guarantee of including all unsafe characters
- No guarantee of future software changes won't introduce new unsafe characters
- Fix:
  - Context specific approach
  - Validate the input is the format of an IP address
  - So much better than the current blocklist
  - Notify the user of invalid input

Least privilege:
- Check processes on web server `ps aux`
- nginx server is runnin as root
- Extremely dangerous
- Would give attacker root privileges
- Server should be running as account with the least privileges possible to serve web pages and any functions required of web applications
- Use 'www-data' since its home directory is already configured with the directory store on all the web pages
- Check files owned by user assigning to web server
- They should be necessary for the web server
- Disable password for web server account `usermod --lock --expiredate 1 www-data`
- /etc/shadow shows us the password authentication is disabled with '!'
- Edit the nginx config file to set the operational user
- Change ownership of the web server files to the new user `chown -R www-data: /var/www`
- Restart the nginx service

Summary:
- Explored commang injection attacks
- Used specialized testing to discover some vulnerabilites
- Covered mitigations types
