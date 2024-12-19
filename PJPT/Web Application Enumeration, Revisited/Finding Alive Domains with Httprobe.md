## Finding Alive Domains with Httprobe
- Sends HTTP and HTTPS requests to list of subdomains
- Any response, no matter the code, says it's alive

### Try it
- Sends both HTTP and HTTPS by defualt:
  
  `cat tesla.com/recon/final.txt | httprobe`
- Set it to only HTTPS:

  `cat tesla.com/recon/final.txt | httprobe -s -p https:443`

### Prepare for Nmap Scan
- Remove protocol and port
  
  `cat tesla.com/recon/final.txt | httprobe -s -p https:443 | sed 's/https\?:\/\///' | tr -d ':443'`
- Add it to script
    ```
    echo "[+] Probing for alive domains..."
    cat $url/recon/final.txt | sort -u | httprobe -s -p https:443 | sed 's/https\?:\/\///' | tr -d ':443' >> $url/recon/alive.txt
    ```
- Run it

  `sudo ./run.sh teslsa.com`

- Check out alive.txt

  `cat tesla.com/recon/alive.txt | grep dev`

  ![image](https://github.com/user-attachments/assets/3f8b2f8d-3d66-4961-aa3f-feb5f9f23095)

### Plan to improve script:
- Take screenshots
  
