# Specialized Testing: Reconnaissance
- This is not part of the Practice Ethical Hacker Course, but I found it relevant to preparing for the PJPT
## Course Overview
- Plan the engagement and gathering information with reconnaissance
- Methods for  mapping the application's attack surface, technologies, and functions
- Process discovering a weide spectrum of vulnerabilities
- Validate findings while demononstrating practical risk through exploitation

- Setup:
  - PractiSec has hired you to conduct a web app pen test of PwnedHub, an application seeking to modernize
  - Join me in the race to find exploitable flaws before PractiSec becomes the next source of free credit monitoring

## Web Application Testing Methodologies
### Web App Pen Testing Introduction
- Web technologies operating over HTTP have become the most widely adopted solution for any system implemting a client server architecture

- Skill Path:
  1. Planning
  2. Gathering Intelligence
  3. Mapping Out Attack Surfaces
  4. Discovering Vulnerabilities
  5. Exploiting Vulnerabilites

### Web App Security Testing Methods
- 3 primary methods of conducting web application security testing
  - DAST: Dyanamic Application Security Testing
    - Tester engages the application from the user's perspective, same as a threat actor
    - Provides an accurate depication of practical risk
  - SAST: Static Application Security Testing
    - Engages the application from the developer's perspective
    - Code specific remediation guidance
  - Hybrid Testing: Combination of DAST and SAST models
    - Does not imply either DAST or SAST are fully applied
    - Allows the tester to focus on strengths by applying components of both
- Seeing things as both a threat actor and a developer reduces trial and error and provides a more accurate assessment of risk
- Pen testing falls under the DAST testing methods

### Dynamic Web Application Security Testing Types
- 3 Primary types of dynamic web application security testing
  - Penetration Testing
    - Threat-oriented test yeilding highly practical results regarding risk
    - Focus on leveraging the application as a means to a specific target or objective
    - Layered defensive are often left in place the simulate the full defensive posture of the target
    - Objective: find an exploitation pathway to the defined target from a specified threat perspective
    - An exercise in reverse engineering that focuses on manipulating inputs and evaluating responses to identify vulnerable behavior
    - Important for a tester to know how web apps are constructed in order to be effective at testing them
  - Vulnerability Assessment
    - A vulnerability-oriented test yeilding theoretical results reguarding risk
    - Layered defenses are removed to prevent interfering with vulnerability discovery
    - Security controls should be left in monitoring mode for data points
    - Objective: vulnerability discovery
    - Often include as much information about the target environment as possible
  - Optimized Pen Testing
    - A pen test where information is exchanged for time
    - Time is the most limiting factor
    - Less practical
    - When you exchange information for time, you exchange practicality for theory
    - Time liminations affect quality of test

### Determining the Dynamic Web Application Security Testing Types
- Each test type serves its own purpose
- Tester should ask the right questions to determine the right test

- Determine to goal of the test
  - Threat oriented
    - No budget: Pen Test
    - Limited budget: Optimized Pen Test
  - Vulnerability oritented: Vulnerability Assessment

### Dynamic Web App Pen Testing Methodology
- Made up of 4 steps
- Iterative and cyclical
- Steps should be done in order and may be done more than one during a test

- Theoretical Methodology:
  1. Reconnaissance
      - Act of gather information about the target from external sources without making direct contact with the target environment
      - Tester can find any number of interesting discoveries
      - Tester should walk away with assumptions about what lies ahead
  2. Mapping
      - Act of learning about the target from a user's perspective
      - Tester gets to know the application and attempts to understand the business purpose it serves
      - Understanding the application is critical because the tester cannot effectively attack what they don't understand
  3. Discovery
      - Act of learning the application from the attacker's perspective
      - Leverages knowledge of the app with malcious intent to identify weaknesses
  4. Exploitation
      - Act of taking vulnerabilities exposed by the app and exploitin them in an effor to reach the stated goal of the test
      - Tester determines the practical risk of everything they have found up to this point

- As the tester moves through the methodology, information builds with each step relying on information gained from the previous
- The information culminates in maximum potential for successful exploitation
- Successful exploitation often leads to exposing new parts of the app that were previously unaccessible
- The methodology become cyclical to appy to the new attack surface

- Prevent breaking out of the protocol:
  - Easy to treat it like a capture the flag
  - Easy to get distracted by vulnerable behavior that appears to jump off the page during early steps
  - Hackers break protocol, go for it
  - Do it with discipline without abandoning the methodology
  - Methodology should be followed as closely as possible
  - However, there is no hard in bouncing between steps
  - 5 for 5 rule:
    - If something grabs the tester's attention...
    1. Make a conscious decision to deviate from the methodology
    2. Spend no more than 5 minutes or 5 attempts tinkering
    3. Document everything done and what to do next
    4. Force to return where left off
    - Prevents rabit holing and chasing red herrings

### Path Scenario
- A company named PractiSec has hired us to test 2 target apps
  - Legacy PwnedHub: A legacy app in a prod environment  
  - PwnedHub 2.0: New version of the legacy in a test env
- Goal: Determine if either app permit un authorized access to sensitive vulnerability information or provide a pivot point into the internal network
- Test is limited by time but no information is provided in exchange
- Dyanmic Web App Pen Test
  - Threat orienteted
  - Focuses on apps in their runtime environment
  - Leverages as a means to a specific target
-  Legacy app has been previously tested
-  Expect but verify vulnerabilities found in legacy app have been addressed in new version
-  Users of the apps are in scope if associated with an app
-  Do not damage the back end database

## Web Application Security Test Planning Process
### The Planing Process
- The planning process is simple with 3 phases
- Begins onces a sales entity initiates a relationship
- Ends with the execution of the test itself

- Phases:
  1. Scoping the engagement
  2. Formalizing the relationship with a contract
  3. Kicking off the engagement

### Scoping Techniques
- The scope of the test is the first thing that must be established
- Often referred to specifically in terms of assets
- Used to determin the cost of the test
- Must define the organizations goals and objectives to accurately forecast a level of effort
- Can be complex

- 2 primary approaches to scoping a web app security test
  - Full coverage
    - Traditional approach
    - Necessary if the org is required to have the target assets fully tested in order to provide an accurate bill of health
    - Usually governed outside of the org's control
    - Falls unger regulatory compliance testing
    - Requires collecting various metrics for the tester to make an accurate assessment of how long it will take to reach the objectives
    - Metrics can be complex and subject to interpretation
    - A desired result can be achived in an indefine number of ways
    - Best to avoid as metrics collection can be innaccurate
  - Best effort
    - Simple and org has full control
    
    - Considerations to provide a reasonable time frame within which the target assets will be tested:
      - Organizational Objectives
      - Availability
      - Budget
      - Asset Quantity
      - Asset Complexity
    - No guarantee of full coverage
    - Best effort is the test, not the scoping
    - The tester will give their best effort given the scope limitations
    - No metric collection
    - Tester gets to work sooner
    - Little uncertainty in costs or effort needed

- Tester Assurance Points:
  - Emphasis on vulnerability categoeires of greatest concern during discovery
  - There is always something to find
  - Provide a follow-up test for full coverage with high accuracy

- Demos are incredibly helpful when scoping a web app security test
- However, discerning the level of effort from a demo requires someone with a lot of testing experience
- Demos will also only allow for scoping what will be seen by the user
- Demos wont consider unlinked content
- Method of testing and type of test will impact how a web app will be scoped
- The very nature of a web app pen test is to find a path to the target, which doesn't require the entire app to be tested
- No matter how scoping is performed, the end result directly affects time and cost, which are highly valued resources
- I is imperative that the scope clearly defines the target and sets realistic expectations for the level of effort that will be accomplished during the test

### Scoping Considerations for Dynamic Environments
- Scoping is fluid and dynamic
- Often leads to more questions as the parameters become more defined
- The environment that is tested can have a major impact on the scope

- Test generally take place in 3 environments:
  - Production
    - Live environment with real users
    - Stable environment that should not change during the test
    - Changes in the code could invalidate all results
    - Automate discovery must be very controlled
    - Most common
  - Test
    - Set up to replicate a prod env
    - An isolated clone removing real world implications
    - Can present issues that leverage third-party service providers
    - Normally stable
    - Generally preferred
  - Development
    - Used by developers to test code changes
    - Unstable, sees rapid, regular changes
    - Does not replicate prod
    - Features and functionality are usually broken
- Access control systems can have a major affect on the scope
- Testing ACS can drastically increase time to meet the objective
- Same for authentication systems
- With an agreed upon scope firmly established and all of our considerations accounted for, we're ready to enter a contractual relationship with the organization

### Contracting
- All about legalities

- Several necessary documents:
  - Non-Disclosure Agreement (NDA)
    - Defines the parameters around sensitive information
    - Inlcudes damages if information is exposed
    - Legally binds each party to protect information
    - Planning process includes sensitive information
  - Master Services Agreement (MSA)
    - Optional document
    - Designed to simplify future contracts within established relationship
    - Frames every contract between both parties
  - Statement of Work (SOW)
    -  Establish scope of work
    -  Legally authorize malicous activities
    -  Set party standard
    -  Specify price
    -  Include MSA items if not present
  -  Rules of Engagement (RoE)
    -  Unique to contracts including security testing
    -  Optional document
    -  Defines the parameters of the test
    -  Intended for the tester

### Kickoff
- A meeting syncronizing everyone's efforts and sets the stage for the upcoming test
- Ensure the prerequisites are tracks
- All stakeholders have needed information

- Contact Plan:
  - Should include information on who to contact
  - When they're available
  - The urgency for when to contact
  - Emails, names, and phone numbers

- Organizational Prerequisites:
  - Provide target information
  - Setup and provide remote access
  - Configure target information
  - Disable or bypass inline security controls
  - Share information for optimization purposes
  - Complete third-party host testing authorization requirements

- Tester Prerequisites:
  - Provide network later information for bypassing inline security controls
  - Provide account information for third-party services such as cloud-based source code repositories
  - Sometimes initiate a background check

- Schedule should be reviewed
- Adjustmens for blackout dates and anticipated downtime
- Last chance to make changes to timeline
- Ends with a set date of when the test should begin

## Methodology: Reconnaissance
### Introduction to Reconnaissance





















