## Wireless Penetration Testing Overview
- Assessment of a wireless network

### Types of networks
- WPA2 PSK
  - PSK = pre-shared key
  - AKA personal
  - Homes
  - Small Business
  - Some medium size business
- WPA2 Enterprise
  - Difficult and costs to setup
  - Medium businesses
  - Large businesses
 
### Activities
- Evaluating strength of PSK
  - Capture handshake
  - Attempt to crack handshake
- Review nearby networks
  - Walkaround
  - AKA wardriving/warwalking
  - Use GPS device
- Assessing guest networks
  - Password?
  - Separation of networks?
  - What can it access?
- Checking network access
  - Are there rogue devices?

### Tools
- Wireless card
  - Used to listen to and inject packets
  - Laptop card isn't strong enough
  - Look for a card compatible with Kali and have both 2.4 and 5 GHz
- Router
  - Not connected to internet
- Laptop
  - Connected to router wirelessly

### Attacking WPA2 PSK
- Place wireless card into monitor mode
- Discover information about network (channel, BSSID)
- Select network and capture data
- Perform deauth attack
- Capture WPA handshake
- Attempt to crack handshake
