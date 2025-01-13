## Manager
1. Can you walk me through the methodology you used to identify vulnerabilities during the technical challenge?

   Definitely! It all revolves around the idea that the better you understand an application, the easier it is you're going to be able to find and exploit vulnerabilities.
   It starts with understanding the application by exploring it's normal functionality to map it.
   I interact with the app as a regular user and I use this opportunity to learn it's features, input fields, and the business purpose it servers.
   Understanding the application is critical because the tester cannot effectively attack what they don't understand.
   From there I learn about the app from the perspective of an attacker.
   This stage is used to discover where the app accepts input to identify weaknesses.
   It requires critical thinking to figure out how the app handles the input and how much it trusts the input.
   With this knowledge, I can then test inputs to cause unintended behavior.
   I look for outputs or responses that indicate unsual responses, errors, and PoCs that the app is vulnerable.
   This stage is used to determine whether the input can alter the intended logic or execution flow.
   While that's a generalized approach, the differences mostly lie in the techniques specific to the context.

2. How do you prioritize vulnerabilities when presenting findings to stakeholders?

   Vulnerabilities are prioritized based on risk and the prioritization can be calculated from the asset category and the vulnerability score.
   So for example, an externally facing app with a high finding has much more risk than an internal server with a moderate finding.
   I scored each finding in the technial challenge with the base CVSS score.
   Some other imporant metrics is the patching maturaty of the assets including how long it will take for the asset to be patched and how well the asset meets patching timelines.
   Prioritization is important so that multiple audiences can understand the impact of the finding.

3. How do you stay proactive and ensure you're up to date with the latest vulnerabilities and attack techniques?

  I stay proactive by dedicating time to learning and staying informed.
  Learning to do things on my own is important skill that can be applied anywhere.
  I consistently spend time learning to improve my understanding and skills by studying for a ceritifation, labs in tryhackme, listening to my coworkers, as well as other experts in the field.
  Reddit security forumns are also interesting to get more indivudal perspectives in a casual environments.
  I reguarly receive emails for news in security, specifically in threats in vulnerabilities, that I enjoy reading. 
  Threat monitoring has always been an exciting resposbibility as an intern because I get to learn about emerging vulnerabilities and understanding their potential risk.
  I've also attended security meetups to hear about interesting topics and network with others in the field.
