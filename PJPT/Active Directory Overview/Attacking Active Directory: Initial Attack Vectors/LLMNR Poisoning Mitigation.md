## LLMNR Poisoning Mitigation

### Disable LLMNR
- The best defense in this case is to disable LLMNR and NBT-NS
- To disable LLMNR, select "Turn OFF Multicast Name Resolution" under Local Computer Policy > Computer Configuration > Administrative Templates > Network > DNS Client in the Group Policy Editor
- To disbale NBT-NS, navigae to Network Connections > Network Adaptor Properties > TCP/IPv4 Properties > Advanced tab > WINS tab and select "Disable NetBIOS over TCP/IP"

### Cannot Disable LLMNR
- Require Network Access Control (only permitted devices on the network)
- Requrie strong user passwords (e.g., >14 character in length and limit common word usage). The more complex and long the password, the harder it is for an attacker to crack the hash
- Password policies should be strong regarless of having LLMNR
