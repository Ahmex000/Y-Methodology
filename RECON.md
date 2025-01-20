
# Recon Methodology

## 1. Select Main Domains & Acquisitions
- Identify the main domains and any acquired domains related to the target.

## 2. Detect Technology
- Use the following command to detect web technologies:
  ```bash
  curl -s -X | grep "<meta>"
  ```

## 3. Virtual Host Fuzzing
- **GoBuster**:
  ```bash
  gobuster vhost -u https://mysite.com -t 50 -w subdomains.txt
  ```
- **VHostScan**:
  ```bash
  VHostScan -t domain.com
  ```

## 4. Resolve Domains to IPs
- **VirusTotal API**:
  ```bash
  curl -s "https://www.virustotal.com/vtapi/v2/domain/report?domain=<DOMAIN>&apikey=<api_key>" | jq -r '.. | .ip_address? // empty' | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}'
  ```
- **Host Command**:
  ```bash
  for url in $(cat grab.txt); do host $url | grep "has address" | cut -d " " -f 4 ;done
  ```

## 5. Claim ASN Number
- **BGP Tools**:
  [BGP Tools](https://bgp.tools/search?q=dell)
- https://blog.voorivex.team/20300-bounties-from-a-200-hour-hacking-challenge
- https://bgp.he.net/ 
- **theHarvester**:
  ```bash
  theHarvester -d target.com -b all
  ```
- **Amass**:
  ```bash
  amass enum -active -d target.com
  ```
- **ASN to Domains**:
  ```bash
  amass intel -asn 12345
  ```

## 6. Convert ASN to CIDR's
- **Whois**:
  ```bash
  whois -h whois.radb.net -- '-i origin AS16509' | grep -Eo "([0-9.]+){4}/[0-9]+" | uniq
  ```
- **ASN To IP's**
  ```bash
  apt-get install whois
  whois -h whois.radb.net  -- '-i origin AS8983' | grep -Eo "([0-9.]+){4}/[0-9]+" | uniq -u > ip_ranges.txt
  ``` 
## 7. Convert CIDR to IP's
- **Nmap**:
  ```bash
  nmap -n -sn 13.35.121.0/24 | grep "for" | cut -d " " -f 5 >> IP.txt
  nmap -n -Pn -sS 13.35.121.0/24 | grep "for" | cut -d " " -f 5 >> IP.txt
  ```

## 8. Resolve IPs to Domains
- **HostHunter**:
  ```bash
  python3 hosthunter.py <target-ips.txt> > vhosts.txt
  ```
- **Nmap**:
  ```bash
  nmap -iL ips.txt -sn | grep for | cut -d " " -f 5
  ```

## **9. Subdomain Enumeration**

### **Tools with Commands**:

#### **1. Subfinder**:
```bash
# Install
go get github.com/subfinder/subfinder

# Basic usage
subfinder -d example.com > example.com.subs

# Recursive
subfinder -d example.com -recursive -silent -t 200 -o example.com.subs

# Use Censys for more results
subfinder -d example.com -b -w wordlist.txt -t 100 -sources censys -set-settings CensysPages=2 -v -o example.com.subs

# Against a list of domains
subfinder -dL domains.txt > example.com.subs
```

#### **2. Amass**:
```bash
# Basic enumeration
amass enum -brute -active -d domain.com -o amass-output.txt

# Against a list of domains
amass enum -df domains.txt -o amass.txt

# Passive enumeration
amass enum -passive -norecursive -noalts -d target.com
```

#### **3. Crt.sh**:
```bash
# Get domains from crt.sh
curl -s "https://crt.sh/?q=%25.$1" | grep -oE "[\.a-zA-Z0-9-]+\.$1" | sort -u

# Certificate Search
curl -s "https://crt.sh/?O=Apple%20Inc.&output=json" | jq -r ".[].common_name" | tr A-Z a-z | unfurl format %r.%t | sort -u | tee apple.cert.txt
```

#### **4. theHarvester**:
```bash
theHarvester -d cisco.com -b all
```

#### **5. SubEnum**:
```bash
# Install
git clone https://github.com/bing0o/SubEnum.git
cd SubEnum
chmod +x setup.sh
./setup.sh

# Basic usage
subenum -d target.com

# Against a list of domains
subenum -l domains.txt -r
```

#### **6. Findomain**:
```bash
findomain -f wilds.txt | tee -a result.txt
findomain -d domain.com | tee -a result.txt
```

#### **7. Assetfinder**:
```bash
# Install
go get -u github.com/tomnomnom/assetfinder

# Basic usage
assetfinder --subs-only <domain>

# Against a list of domains
cat ../target | xargs -I {} assetfinder --subs-only {} >> assetfinder
```

#### **8. Sublist3r**:
```bash
sublist3r -d example.com -o subdomains.txt
```

#### **9. Massdns**:
```bash
massdns -r resolvers.txt -t A -o S -w massdns_output.txt domains.txt
```

#### **10. Shodan**:
```bash
# Search for subdomains using Shodan CLI
shodan domain <domain>
```

#### **11. VirusTotal**:
```bash
curl -s "https://www.virustotal.com/vtapi/v2/domain/report?apikey=<api_key>&domain=<DOMAIN>" | jq -r '.domain_siblings[]'
```

#### **12. Favicon Hash**:
```bash
cat my_targets.txt | xargs -I %% bash -c 'echo "http://%%/favicon.ico"' > targets.txt
python3 favihash.py -f https://target/favicon.ico -t targets.txt -s
```

#### **13. Chaos**:
```bash
chaos -d example.com -o chaos.txt
```

#### **14. SecurityTrails**:
```bash
curl -s "https://api.securitytrails.com/v1/domain/<DOMAIN>/subdomains?apikey=<API_KEY>"
```

#### **15. Spyse**:
```bash
spyse -t domain -q example.com
```

#### **16. URLScan**:
```bash
urlscan -d example.com
```

#### **17. ZoomEye**:
```bash
zoomeye search "domain:example.com"
```

#### **18. Censys**:
```bash
censys search "parsed.names: example.com" --index certificates
```

#### **19. DNS Recon**:
```bash
dnsrecon -d example.com -t brt -D wordlist.txt -c dnsrecon_output.csv
```

#### **20. Knock**:
```bash
knockpy example.com
```

#### **21. Frogy**:
```bash
frogy -d example.com -o frogy_output.txt
```

#### **22. GitHub Subdomains**:
```bash
github-subdomains -d example.com -t <github_token> -o github_subs.txt
```

#### **23. GitLab Subdomains**:
```bash
gitlab-subdomains -d example.com -t <gitlab_token> -o gitlab_subs.txt
```

#### **24. Alterx**:
```bash
alterx -l domains.txt -o alterx_output.txt
```

#### **25. OneForAll**:
```bash
python3 oneforall.py --target example.com run
```

#### **26. DomainCollector**:
```bash
domainCollector -d example.com -o domainCollector_output.txt
```

---

### **Tools without Commands (Web Tools or No Direct Commands)**:
1. [Subdomain Radar](https://subdomainradar.io/)
2. [DNSDumpster](https://dnsdumpster.com/)
3. [Netlas.io](https://app.netlas.io/)
4. [GitHub Subdomains](https://github.com/gwen001/github-subdomains)
5. [GitLab Subdomains](https://github.com/gwen001/gitlab-subdomains)
6. [PureDNS](https://github.com/d3mondev/puredns)
7. [BBOT](https://github.com/blacklanternsecurity/bbot)
8. [OneForAll](https://github.com/shmilylty/OneForAll)
9. [DomainCollector](https://github.com/Cyber-Guy1/domainCollector)

---

## **9.1. Active Subdomain Enumeration**
- **PureDNS**:
  ```bash
  puredns bruteforce all.txt domain.com
  puredns bruteforce all.txt -d domains.txt
  ```

---

## **10. Subdomain Brute Forcing**
- **ShuffleDNS**:
  ```bash
  shuffledns -d example.com -w wordlist.txt -r resolvers.txt -o output.txt
  ```

- **Gobuster**:
  ```bash
  gobuster dns -d mysite.com -t 50 -w common-names.txt
  ```

---

## 11. Directory Busting
- **Dirsearch**:
  ```bash
  python3 dirsearch.py -u https://target.com -e php,html,js
  ```
- **Feroxbuster**:
  ```bash
  feroxbuster -u https://target.com -w /path/to/wordlist
  ```
- **FFuf**:
  ```bash
  ffuf -u https://target.com/FUZZ -w wordlist.txt -o ffuf_results.json
  ```

## 12. Dorking
- **Tools**:
  - [Searx](https://searx.thegpm.org/)
  - [LostSec](https://lostsec.xyz/)
  - [Leak-Lookup](https://leak-lookup.com/account/login)
  - [Dehashed](https://www.dehashed.com/)
  - [Wayback Machine](https://web.archive.org/cdx/search?url=simplisafe.com/&matchType=domain&collapse=urlkey&output=text&fl=original&filter=mimetype:application/x-shockwave-flash&limit=100000&_=1507209148310)
  - [Pentest-Tools](https://pentest-tools.com/information-gathering/google-hacking)
  - [Exploit-DB](https://www.exploit-db.com/google-hacking-database)
  - [Hunter](https://hunter.how/list?searchValue=product.name%3D%22Zabbix%22)
  - [RocketReach](https://rocketreach.co/person?start=1&pageSize=10&link=https://www.linkedin.com/in/brianarsenault)
  - [Dork Helper](https://www.lopseg.com.br/dork-helper)
  - [ProxyNova](https://www.proxynova.com/tools/comb/)
  - [Domain Glass](https://domain.glass/)
  - [ZoomEye](https://www.zoomeye.ai/)
  - [Dorks](https://dorks.faisalahmed.me/)
  - [Criminal IP](https://www.criminalip.io/)
  - censys
  - bevigil
  - binaryedge
  - cerspotter
  - whoisxmlapi
  - fofa
  - shodan
  - github
  - virustotal
  - zoomeye

 
    
- **GooFuzz**:
  ```bash
  GooFuzz -t nasa.gov -e pdf,bak,old -d 10
  GooFuzz -t nasa.gov -e wordlists/extensions.txt -d 30
  GooFuzz -t nasa.gov -w wordlists/words-100.txt -p 3
  ```
- **Favicon Search**
```bash
  import hashlib
  def calculate_favicon_hash(file_path):
    with open(file_path, 'rb') as file:
        favicon_data = file.read()
        favicon_hash = hashlib.md5(favicon_data).hexdigest()
    return favicon_hash
  favicon_path = '/path/to/favicon.ico'
  favicon_hash = calculate_favicon_hash(favicon_path)
  print(favicon_hash)
```
- http.favicon.hash:[Favicon hash here]
 ```bash
  cat urls.txt | python3 favfreak.py -o output
  http.favicon.hash:-<hash>
  ```

## 13. Test for Subdomain Takeover
- **Subzy**:
  ```bash
  subzy run --targets subdomains.txt --concurrency 100 --hide_fails --verify_ssl
  ```

## 14. Live Subdomains
- **Httprobe**:
  ```bash
  cat subs.txt | httprobe
  ```
- **Httpx**:
  ```bash
  cat domains.txt | httpx -sc -ip -server -title -wc
  ```

## 15. Port Scanning
- **Naabu**:
  ```bash
  naabu -host <ip> -p- -Pn -o portscan | httpx -sc -td -server
  ```
- **Nmap**:
  ```bash
  nmap -sV -sC -sS 87.251.41.115
  ```

## 16. Claim URLs
- **Waybackurls**:
  ```bash
  echo "example.com" | waybackurls | grep -iE '\.js' | grep -ivE '\.json' | sort -u > j.txt
  ```
- **Gospider**:
  ```bash
  gospider -s "https://google.com/" -o output -c 10 -d 1
  ```
- **Katana**:
  ```bash
  katana -u https://tesla.com
  ```

## 17. Scan JS-Files
- **LinkFinder**:
  ```bash
  python setup.py install
  ```
- **DumpsterDiver**:
  ```bash
  python3 DumpsterDiver.py
  ```

## 18. Hidden Parameters
- **FFuf**:
  ```bash
  ffuf -w /path/to/paramnames.txt -u https://target/script.php?FUZZ=test_value -fs <Number of Default Length>
  ```
- **Arjun**:
  ```bash
  arjun -u https://api.example.com/endpoint
  ```

## 19. Employee Enumeration
- **GitHub**:
  Use GitHub dorks to find employee information.

## 20. S3 Bucket Enumeration
- **S3Scanner**:
  ```bash
  s3scanner -l domains.txt
  ```

## 21. Subdomain Monitoring
- **Facebook CT Search**:
  [Facebook CT Search](https://developers.facebook.com/tools/ct/search/)

## 22. Nuclei
- **Nuclei**:
  ```bash
  nuclei -l -t ~/nuclei-templates/http/exposures/
  ```

## 23. Test for XSS, Open Redirect, LFI, SSTI, SQL Injection, CORS
- **XSS Test**:
  ```bash
  go run main.go xss.txt
  ```
- **CORS**:
  ```bash
  assetfinder http://fitbit.com | httpx -threads 300 -follow-redirects -silent | rush -j200 'curl -m5 -s -I -H "Origin: http://evil.com" {} | [[ $(grep -c "http://evil.com") -gt 0 ]] && printf "\n\033[0;32m[VUL TO CORS] \033[0m{}"' 2>/dev/null
  ```

---

## Additional Tools
- [Offensive Tools](https://offsec.tools/)

---

- **Get Target Real IP**: [DNS Checker](https://dnschecker.org/ip-location.php?ip=147.154.104.158)
```
