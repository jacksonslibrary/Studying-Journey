## Finding Subdomains with Assetfinder
- Assetfinder is a great tool at finding subdomains
- Install the tool

  `go install github.com/tomnomnom/assetfinder@latest`

- Let's try it

  `assetfinder tesla.com`

- Can find a lot of subdomains very quickly
- Time to automate it

  `vim run.sh`

  ```
  #!/bin/bash
  url=$1
  
  if [ ! -d "$url" ];then
    mkdir $url
  fi
  
  if [ ! -d "$url/recon" ];then
    mkdir $url/recon
  fi
  
  echo "[+] Harvesting subdomains with assetfinder..."
  assetfinder $url >> $url/recon/assets.txt
  cat $url/recon/assets.txt | grep $1 >> $url/recon/final.txt
  rm $url/recon/assets.txt
  ```
  - Makes a couple directories if they don't exist
  - Run Assetfinder, putting findings in assets.txt
  - Put the assets that match the url in final.txt
  - Then removes assets.txt
- Add execution permission

  `chmod +x run.sh`

- Run it

  `sudo ./run.sh tesla.com`

- cd into directory

  `cd tesla.com/recon`

  `cat final.txt`

![image](https://github.com/user-attachments/assets/a5d5f810-48b2-49cc-af0a-7d84775bfd3e)

- Plan to improve script:
  - Remove duplicates
  - Check which ones are alive
