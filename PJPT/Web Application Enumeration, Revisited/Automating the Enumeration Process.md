## Automating the Enumeration Process 
- Adding to the script:
  ```
  echo "[+] Checking for possible subdomain takeover..."
 
  if [ ! -f "$url/recon/potential_takeovers/potential_takeovers.txt" ];then
  	touch $url/recon/potential_takeovers/potential_takeovers.txt
  fi
   
  subjack -w $url/recon/final.txt -t 100 -timeout 30 -ssl -c ~/go/src/github.com/haccer/subjack/fingerprints.json -v 3 -o $url/recon/potential_takeovers/potential_takeovers.txt
   
  echo "[+] Scanning for open ports..."
  nmap -iL $url/recon/httprobe/alive.txt -T4 -oA $url/recon/scans/scanned.txt
   
  echo "[+] Scraping wayback data..."
  cat $url/recon/final.txt | waybackurls >> $url/recon/wayback/wayback_output.txt
  sort -u $url/recon/wayback/wayback_output.txt
   
  echo "[+] Pulling and compiling all possible params found in wayback data..."
  cat $url/recon/wayback/wayback_output.txt | grep '?*=' | cut -d '=' -f 1 | sort -u >> $url/recon/wayback/params/wayback_params.txt
  for line in $(cat $url/recon/wayback/params/wayback_params.txt);do echo $line'=';done
   
  echo "[+] Pulling and compiling js/php/aspx/jsp/json files from wayback output..."
  for line in $(cat $url/recon/wayback/wayback_output.txt);do
  	ext="${line##*.}"
  	if [[ "$ext" == "js" ]]; then
  		echo $line >> $url/recon/wayback/extensions/js1.txt
  		sort -u $url/recon/wayback/extensions/js1.txt >> $url/recon/wayback/extensions/js.txt
  	fi
  	if [[ "$ext" == "html" ]];then
  		echo $line >> $url/recon/wayback/extensions/jsp1.txt
  		sort -u $url/recon/wayback/extensions/jsp1.txt >> $url/recon/wayback/extensions/jsp.txt
  	fi
  	if [[ "$ext" == "json" ]];then
  		echo $line >> $url/recon/wayback/extensions/json1.txt
  		sort -u $url/recon/wayback/extensions/json1.txt >> $url/recon/wayback/extensions/json.txt
  	fi
  	if [[ "$ext" == "php" ]];then
  		echo $line >> $url/recon/wayback/extensions/php1.txt
  		sort -u $url/recon/wayback/extensions/php1.txt >> $url/recon/wayback/extensions/php.txt
  	fi
  	if [[ "$ext" == "aspx" ]];then
  		echo $line >> $url/recon/wayback/extensions/aspx1.txt
  		sort -u $url/recon/wayback/extensions/aspx1.txt >> $url/recon/wayback/extensions/aspx.txt
  	fi
  done
   
  rm $url/recon/wayback/extensions/js1.txt
  rm $url/recon/wayback/extensions/jsp1.txt
  rm $url/recon/wayback/extensions/json1.txt
  rm $url/recon/wayback/extensions/php1.txt
  rm $url/recon/wayback/extensions/aspx1.txt
  ```

  - Probes for alive domains using httprobe and saves results in recon/httprobe/alive.txt
  - Checks for potential subdomain takeovers using subjack and saves results in recon/potential_takeovers/potential_takeovers.txt
  - Scans alive domains for open ports using nmap and saves results in recon/scans/scanned.txt
  - Scrapes Wayback Machine data for discovered subdomains using waybackurls and saves results in recon/wayback/wayback_output.txt
  - Extracts and compiles URL parameters from Wayback data into recon/wayback/params/wayback_params.txt
  - Compiles JavaScript, HTML, JSON, PHP, and ASPX file URLs from Wayback data into respective files in recon/wayback/extensions/
  - Cleans up temporary files created during the extension compilation step (js1.txt, jsp1.txt, json1.txt, php1.txt, aspx1.txt)

### Plan:
- Get into web app pentesting
- Talk about OWASP Top 10
- Burp Suite at deeper level
- Top 10 attacks
