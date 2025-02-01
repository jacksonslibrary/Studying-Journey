##  LLMNR Poisoning Overview
- Very common and popular attack for internal pen tests
- Link Local Multicast Name Resolution Poisoning

### What is LLMNR
- Used to identify hosts when DNS fails to do so
- Previously NBT-NS
- Key flaw is that the services utilize a user's username and NTLMv2 hash when appropriately responded to
- When we respond to this traffic, now this is a man in the middle attack

### Attack Overview
- Victim wants to reach out to a shared folder 'hackme'
- Mistype hackme as hackm
- Server does not know a hackm
- Victim then requests hackm from the network
- An attacker can intercept this request and respond to it
- Attack's price for the share is the victim's hash
- If their hash is weak enough, it can be cracked offline

  ![image](https://github.com/user-attachments/assets/548c109b-9a48-431e-b273-ea0087c97d8b)

### Attack Stages
Step 1: Run Responder
- `sudo responder -I tun0 -dwP`
- Run it early in the morning or after lunch when people are logging into their computers
- Good idea to also run Nessus scans at the same time
- Should be familiar with because it does a few different attacks

Step 2: An Event Occurs
- Try to go to a file share that really points to attacker's IP address
- In theory, we're actually pointing ourselves at responder

Step 3: Get Dem Hashes
- Responder collects hash

Step 4: Crack Dem Hashes
- Take the hash offline and crack it

### Summary
- LLMNR is 'a feature, not a bug'
- Quick wins for an internal pen test
