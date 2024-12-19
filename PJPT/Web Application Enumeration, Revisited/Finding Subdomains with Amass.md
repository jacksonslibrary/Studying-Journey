## Finding Subdomains with Amass
- Amass is also used for subdomain hunting
- Every tool is a little different in how it pulls subdomains
- More tools means more subdomains
- Best to cover entire scope of bug bounty or pentest

### Try it
- Comes installed with Kali

  `amass enum -d tesla.com`

- Add it to the script

  ```
  echo "[+] Harvesting subdomains with Amass..."
  amass enum -d $url >> $url/recon/f.txt
  sort -u $url/recon/f.txt >> $url/recon/final/txt
  rm $url/recon/f.txt
  ```
    - Runs Amass on the target, putting findings in f.txt
    - Sorts findings into final.txt
    - Then removes unsorted file

  ### Plan to improve script:
- Probe for alive subdomains
- Save a lot of time
