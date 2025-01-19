Recon Methodology

1. Select Main Domains & Acquisitions
   - Identify the main domains and any acquired domains related to the target.

2. Detect Technology
   - Use the following command to detect web technologies:
     curl -s -X | grep "<meta>"

3. Virtual Host Fuzzing
   - GoBuster:
     gobuster vhost -u https://mysite.com -t 50 -w subdomains.txt
   - VHostScan:
     VHostScan -t domain.com

4. Resolve Domains to IPs
   - VirusTotal API:
     curl -s "https://www.virustotal.com/vtapi/v2/domain/report?domain=<DOMAIN>&apikey=<api_key>" | jq -r '.. | .ip_address? // empty' | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}'
   - Host Command:
     for url in $(cat grab.txt); do host $url | grep "has address" | cut -d " " -f 4 ;done

5. Claim ASN Number
   - BGP Tools:
     https://bgp.tools/search?q=dell
   - theHarvester:
     theHarvester -d target.com -b all
   - Amass:
     amass enum -active -d target.com
   - ASN to Domains:
     amass intel -asn 12345

6. Convert ASN to CIDR's
   - Whois:
     whois -h whois.radb.net -- '-i origin AS16509' | grep -Eo "([0-9.]+){4}/[0-9]+" | uniq

7. Convert CIDR to IP's
   - Nmap:
     nmap -n -sn 13.35.121.0/24 | grep "for" | cut -d " " -f 5 >> IP.txt
     nmap -n -Pn -sS 13.35.121.0/24 | grep "for" | cut -d " " -f 5 >> IP.txt

8. Resolve IPs to Domains
   - HostHunter:
     python3 hosthunter.py <target-ips.txt> > vhosts.txt
   - Nmap:
     nmap -iL ips.txt -sn | grep for | cut -d " " -f 5

9. Subdomain Enumeration
   - Tools:
     - https://subdomainradar.io/
     - https://github.com/projectdiscovery/alterx
     - https://github.com/gwen001/github-subdomains
     - https://github.com/d3mondev/puredns
     - https://github.com/blacklanternsecurity/bbot
     - https://github.com/shmilylty/OneForAll
     - https://github.com/guelfoweb/knock
     - https://github.com/iamthefrogy/frogy
     - https://github.com/Cyber-Guy1/domainCollector
   - VirusTotal:
     curl -s "https://www.virustotal.com/vtapi/v2/domain/report?apikey=<api_key>&domain=<DOMAIN>" | jq -r '.domain_siblings[]'
   - Subfinder:
     subfinder -d example.com > example.com.subs
     subfinder -d example.com -recursive -silent -t 200 -o example.com.subs
     subfinder -d example.com -b -w wordlist.txt -t 100 -sources censys -set-settings CensysPages=2 -v -o example.com.subs
     subfinder -dL domains.txt > example.com.subs
   - CRT.sh:
     curl -s "https://crt.sh/?q=%25.$1" | grep -oE "[\.a-zA-Z0-9-]+\.$1" | sort -u
   - Amass:
     amass enum -brute -active -d domain.com -o amass-output.txt
     amass enum -df domains.txt -o amass.txt
   - Favicon:
     cat my_targets.txt | xargs -I %% bash -c 'echo "http://%%/favicon.ico"' > targets.txt
     python3 favihash.py -f https://target/favicon.ico -t targets.txt -s
   - theHarvester:
     theHarvester -d cisco.com -b all
   - SubEnum:
     subenum -d target.com
     subenum -l domains.txt -r
   - Findomain:
     findomain -f wilds.txt | tee -a result.txt
     findomain -d domain.com | tee -a result.txt
   - Assetfinder:
     assetfinder --subs-only <domain>
     cat ../target | xargs -I {} assetfinder --subs-only {} >> assetfinder

10. Subdomain Brute Forcing
    - PureDNS:
      puredns bruteforce all.txt domain.com
      puredns bruteforce all.txt -d domains.txt
    - Gobuster:
      gobuster dns -d mysite.com -t 50 -w common-names.txt

11. Directory Busting
    - Dirsearch:
      python3 dirsearch.py -u https://target.com -e php,html,js
    - Feroxbuster:
      feroxbuster -u https://target.com -w /path/to/wordlist
    - FFuf:
      ffuf -u https://target.com/FUZZ -w wordlist.txt -o ffuf_results.json

12. Dorking
    - Tools:
      - https://searx.thegpm.org/
      - https://lostsec.xyz/
      - https://leak-lookup.com/account/login
      - https://www.dehashed.com/
      - https://web.archive.org/cdx/search?url=simplisafe.com/&matchType=domain&collapse=urlkey&output=text&fl=original&filter=mimetype:application/x-shockwave-flash&limit=100000&_=1507209148310
      - https://pentest-tools.com/information-gathering/google-hacking
      - https://www.exploit-db.com/google-hacking-database
      - https://hunter.how/list?searchValue=product.name%3D%22Zabbix%22
      - https://rocketreach.co/person?start=1&pageSize=10&link=https://www.linkedin.com/in/brianarsenault
      - https://www.lopseg.com.br/dork-helper
      - https://www.proxynova.com/tools/comb/
      - https://domain.glass/
      - https://www.zoomeye.ai/
      - https://dorks.faisalahmed.me/
      - https://www.criminalip.io/
    - GooFuzz:
      GooFuzz -t nasa.gov -e pdf,bak,old -d 10
      GooFuzz -t nasa.gov -e wordlists/extensions.txt -d 30
      GooFuzz -t nasa.gov -w wordlists/words-100.txt -p 3

13. Test for Subdomain Takeover
    - Subzy:
      subzy run --targets subdomains.txt --concurrency 100 --hide_fails --verify_ssl

14. Live Subdomains
    - Httprobe:
      cat subs.txt | httprobe
    - Httpx:
      cat domains.txt | httpx -sc -ip -server -title -wc

15. Port Scanning
    - Naabu:
      naabu -host <ip> -p- -Pn -o portscan | httpx -sc -td -server
    - Nmap:
      nmap -sV -sC -sS 87.251.41.115

16. Claim URLs
    - Waybackurls:
      echo "example.com" | waybackurls | grep -iE '\.js' | grep -ivE '\.json' | sort -u > j.txt
    - Gospider:
      gospider -s "https://google.com/" -o output -c 10 -d 1
    - Katana:
      katana -u https://tesla.com

17. Scan JS-Files
    - LinkFinder:
      python setup.py install
    - DumpsterDiver:
      python3 DumpsterDiver.py

18. Hidden Parameters
    - FFuf:
      ffuf -w /path/to/paramnames.txt -u https://target/script.php?FUZZ=test_value -fs <Number of Default Length>
    - Arjun:
      arjun -u https://api.example.com/endpoint

19. Employee Enumeration
    - GitHub:
      Use GitHub dorks to find employee information.

20. S3 Bucket Enumeration
    - S3Scanner:
      s3scanner -l domains.txt

21. Subdomain Monitoring
    - Facebook CT Search:
      https://developers.facebook.com/tools/ct/search/

22. Nuclei
    - Nuclei:
      nuclei -l -t ~/nuclei-templates/http/exposures/

23. Test for XSS, Open Redirect, LFI, SSTI, SQL Injection, CORS
    - XSS Test:
      go run main.go xss.txt
    - CORS:
      assetfinder http://fitbit.com | httpx -threads 300 -follow-redirects -silent | rush -j200 'curl -m5 -s -I -H "Origin: http://evil.com" {} | [[ $(grep -c "http://evil.com") -gt 0 ]] && printf "\n\033[0;32m[VUL TO CORS] \033[0m{}"' 2>/dev/null

Additional Tools
- Offensive Tools: https://offsec.tools/

----------------------------------------

- get target real IP => https://dnschecker.org/ip-location.php?ip=147.154.104.158


