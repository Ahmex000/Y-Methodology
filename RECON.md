
# Recon Methodology

## 1. Select Main Domains & Acquisitions
- Identify the main domains and any acquired domains related to the target.

--- 

## 3. Virtual Host Fuzzing (search for another domains in the same IP)
- **GoBuster**:
  ```bash
  gobuster vhost -u https://mysite.com -t 50 -w subdomains.txt
  ```
- **VHostScan**:
  ```bash
  VHostScan -t domain.com
  ```
- **Reverse WHOIS (Do revers WHO is For target Domain)**:
  ```bash
  amass intel -d domain.com -whois
  ```
 ```bash
 https://who.is/
  ```
--- 

- **search for TLD's to another TLD**

```
echo "example.com" | gotator -tld

=>
example.net
example.org
example.io
example.co
example.tech

- SecurityTrails

curl -H "APIKEY: YOUR_API_KEY" "https://api.securitytrails.com/v1/domain/example.com/list"

- Subfinder

subfinder -d example.com -all -o tlds_results.txt

```


## 4. Reverse DNS Resolve Domains to IPs (don't forget to filter in scope domains )

- **Get Target Real IP**: [DNS Checker](https://dnschecker.org/ip-location.php?ip=147.154.104.158)
- **VirusTotal API**:
  ```bash
  curl -s "https://www.virustotal.com/vtapi/v2/domain/report?domain=<DOMAIN>&apikey=<api_key>" | jq -r '.. | .ip_address? // empty' | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}'
  ```
- **zdns**:
  ```bash
   git clone https://github.com/zmap/zdns.git
   cd zdns
   make install
   -----
   echo "censys.io" | zdns A
  ```
  
- **Host Command**:
  ```bash
  for url in $(cat grab.txt); do host $url | grep "has address" | cut -d " " -f 4 ;done
  ```
  
--- 
- **Reverse DNS with commands**
```bash
dig -x 8.8.8.8 +short
host 8.8.8.8
=>
8.8.8.8.in-addr.arpa domain name pointer dns.google.
curl -s "https://api.hackertarget.com/reverseiplookup/?q=8.8.8.8"
=>
example.com
sub.example.com
mail.example.com
vpn.example.com

```




## 5. Claim ASN Number (claim ASN's For Domains)
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
- **check if tjhis ASN for Target Scope**:
  ```bash
  amass intel -asn Found ASN 
  - rpeonse will contain ASN owner name 
  ```  
- **ASN to Domains**:
  ```bash
  amass intel -asn 12345
  ```
  ### 5.1 Claim CIDR's Number (claim CIDR's For Domains)
- https://bgp.he.net/


--- 


## 6. Convert ASN to CIDR's (conver ASN number to CIDR and ASN to IP's)
- **Whois**:
  ```bash
  whois -h whois.radb.net -- '-i origin AS16509' | grep -Eo "([0-9.]+){4}/[0-9]+" | uniq
  ```
- **ASN To IP's**
  ```bash
  apt-get install whois
  whois -h whois.radb.net  -- '-i origin AS8983' | grep -Eo "([0-9.]+){4}/[0-9]+" | uniq -u > ip_ranges.txt
  ```
  
--- 


## 7. Convert CIDR to IP's
- **Nmap**:
  ```bash
  nmap -n -sn 13.35.121.0/24 | grep "for" | cut -d " " -f 5 >> IP.txt
  nmap -n -Pn -sS 13.35.121.0/24 | grep "for" | cut -d " " -f 5 >> IP.txt
  ```
- **Scan CIDR open Ports**:
  ```bash
  https://github.com/zmap/zmap?tab=readme-ov-file
  ```
  
--- 



## 8. Resolve IPs to Domains (Reverse IP)
- **HostHunter**:
  ```bash
  python3 hosthunter.py <target-ips.txt> > vhosts.txt
  ```
- **Nmap**:
  ```bash
  nmap -iL ips.txt -sn | grep for | cut -d " " -f 5
  ```

  - **Reverse DNS Lookup**:
  ```bash
  Reverse DNS Lookup
  ```
  - **zdns Reverse DNS Lookup**:
  ```bash
   git clone https://github.com/zmap/zdns.git
   cd zdns
   make install
   -----
   echo "censys.io" | zdns A
  ```

  ```bash
  curl -s "https://api.hackertarget.com/reverseiplookup/?q=8.8.8.8"
  =>
  example.com
  sub.example.com
  mail.example.com
  vpn.example.com
  
  - shodan
    shodan search "net:8.8.8.8"
  
  - Censys
  censys search "ip:8.8.8.8"
  
  - SecurityTrails
  curl -H "APIKEY: YOUR_API_KEY" "https://api.securitytrails.com/v1/domain/example.com/subdomains"

  ```

  ### **8.1 sacn all web IPs to search for target CN/SAN's**
  ```bash
  - just use https://github.com/g0ldencybersec/CloudRecon to scan all web ips form https://kaeferjaeger.gay/
  - search for target CN/SAN
  ```

--- 


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
- please dont forget to add api keys for following API's
```bash
BeVigil, BinaryEdge, BufferOver, C99, Censys, CertSpotter, Chaos, Chinaz, DNSDB, Fofa, FullHunt, GitHub, Intelx, PassiveTotal, quake, Robtex, SecurityTrails, Shodan, ThreatBook, VirusTotal, WhoisXML API, ZoomEye API china - worldwide, dnsrepo, Hunter, Facebook, BuiltWith
```
- **see** subfinder Doc's https://docs.projectdiscovery.io/tools/subfinder/install

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

#### **27. DomainCollector**:
```bash
domainCollector -d example.com -o domainCollector_output.txt
```

#### **27. new subdomains not saved in DNS servers**:
```bash
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -subject -issuer -ext subjectAltName
```

#### **28. Add and Analytics Relationships**:

```
## **Ad & Analytics Relationships Recon**  

### **Why?**  
- Find hidden domains linked to the same company.  
- Identify **staging/dev environments** with weaker security.  
- Use analytics & ad tracking IDs to **correlate domains**.  

### **Google Analytics ID Lookup**  
Extract GA ID and find related domains:  
```sh
curl -s https://example.com | grep -oE "UA-[0-9]+-[0-9]+"
Search for domains using the same ID:
🔗 https://builtwith.com/

Facebook Pixel ID Lookup
Extract Facebook Pixel ID:

sh
Copy
Edit
curl -s https://example.com | grep -oE "fbq\('init', '[0-9]+'\)"
Find related domains:
🔗 https://www.whatsmyserp.com/tools/facebook-pixel-id-lookup

Google AdSense ID Lookup
Extract AdSense Publisher ID:

sh
Copy
Edit
curl -s https://example.com | grep -oE "pub-[0-9]+"
Find other sites using the same AdSense ID:
🔗 https://spyonweb.com/
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
10. [Crobat](https://github.com/Cgboal/SonarSearch)
11. https://github.com/screetsec/Sudomy
12. https://github.com/Fadavvi/Sub-Drill

---

### **9.1. Active Subdomain Enumeration**
- **PureDNS**:
  ```bash
  puredns bruteforce all.txt domain.com
  puredns bruteforce all.txt -d domains.txt
  ```

### **9.2 sub enum via Burp Pro**

### **9.3 sub enum via browser extention**
- https://github.com/hackersthan/grepsubsfromwebpages // don't forget to creat CLI tool to do same thing's , and use it in you'r endpoint's
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
- **DNSCAN**
  ```bash
  https://github.com/rbsec/dnscan
  $ dnscan.py -d dev-%%.example.org
  ```
- https://wordlists-cdn.assetnote.io/data/manual/2m-subdomains.txt
- https://wordlists-cdn.assetnote.io/data/manual/best-dns-wordlist.txt 
  
---

## **Permutation Scanning – Discovering Hidden Subdomains**  

### **Why?**  
- Finds **hidden subdomains** not listed in DNS records.  
- Identifies **development, testing, and internal environments**.  
- Detects **common naming patterns** like `dev.example.com`, `vpn.example.com`.  
- Expands the attack surface by uncovering **misconfigured subdomains**.  

### **1️⃣ Generating Permutations with `gotator`**  
```sh
gotator -sub subdomains.txt -perm mutations.txt -depth 2 -numbers 10 -mindup -adv -out output.txt
```
**Example Output:**  
```
dev.example.com  
test.example.com  
vpn.example.com  
staging.example.com  
```

### **2️⃣ Creating Smart Permutations with `dnsgen`**  
```sh
cat subdomains.txt | dnsgen - | tee permutations.txt
```
**Example Output:**  
```
admin.example.com  
secure.example.com  
mail.example.com  
legacy.example.com  
```

### **3️⃣ Testing Permutations with `altdns`**  
```sh
altdns -i subdomains.txt -o permutations.txt -w words.txt -r -s results.txt
```
**Example Output:**  
```
vpn.example.com - FOUND  
internal.example.com - FOUND  
db.example.com - NOT FOUND  
```

### **4️⃣ Validating Results with `massdns` or `puredns`**  
```sh
puredns resolve permutations.txt -w resolvers.txt -o valid_subdomains.txt
```

### **Key Takeaways:**  
✅ **Start with known subdomains** using tools like `Amass` and `Subfinder`.  
✅ **Use permutation tools** (`gotator`, `dnsgen`, `altdns`) to generate subdomains.  
✅ **Validate results** using DNS resolvers like `massdns` or `puredns`.  
✅ **Analyze found subdomains** for potential misconfigurations and vulnerabilities.  

## 2. Detect Technology for every subdomain
- Use the following command to detect web technologies:
  ```bash
  curl -s -X | grep "<meta>" //look if the site is wordpress
  ```
- dnsdumpster
  ```bash
  https://dnsdumpster.com/
  ```
- **WhatWeb**:
  ```bash
  whatweb <target-url>
  ```

- **Wappalyzer**:
  ```bash
  wappalyzer <target-url>
  ```

- **BuiltWith**:
  ```bash
  builtwith <target-url>
  ```

- **Netcraft**:
  ```bash
  netcraft <target-url>
  ```

- **Nmap (With HTTP/HTTPS scripts)**:
  ```bash
  nmap -sV --script=http-enum <target-url>
  ```

- **HTTP Headers Inspection**:
  ```bash
  curl -I <target-url>
  ```

- **Shodan**:
  ```bash
  shodan host <target-ip>
  ```

- **JSFiddle & Web Technologies by URL**:
  ```bash
  # Use JSFiddle or other tools to inspect source code or URLs to identify JavaScript frameworks.
  ```

- **WhatRuns**:
  ```bash
  # Install browser extension to detect technologies used by the website.
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

--- 


## 12. Dorking

---

### **$3 Bucket Recon Methods**

1. **Google Dork to Find $3 Buckets**
   - `site:s3.amazonaws.com site.com`
   - `site:amazonaws.com inurl:s3.amazonaws.com`
   - `site:s3.amazonaws.com intitle:index.of bucket`
   - https://github.com/initstring/cloud_enum

2. **Using Burp Suite**
   - Crawl the application through a browser proxy.
   - Use Burp Suite’s sitemap feature to discover $3 buckets.
   - Look for URLs or headers mentioning `s3.amazonaws.com` or `x-amz-bucket`.

3. **From Application**
   - Right-click on an image in the application and open it in a new tab.
   - Check if the URL format is `https://name.s3.amazonaws.com/image1.png`. The `name` before `.s3` is the bucket name.

4. **Online Tools on GitHub**
   - [S3Scanner](https://github.com/sa7mon/S3Scanner)
   - [Mass3](https://github.com/smiegles/mass3)
   - [slurp](https://github.com/3xbarath/slurp)
   - [lazyS3](https://github.com/nahamsec/lazys3)
   - [bucket_finder](https://github.com/msttwidmer/bucket_finder)
   - [ANSBucketDump](https://github.com/netgusto/ANSBucketDump)
   - [sandcastle](https://github.com/DxSearches/sandcastle)
   - [DumpsterDiver](https://github.com/securing/DumpsterDiver)
   - [$3 Bucket Finder](https://github.com/gwen001/s3-buckets-finder)
   - [S3Scanner](https://github.com/sa7mon/S3Scanner)
   - [Mass3](https://github.com/smiegles/mass3)
   - [slurp](https://github.com/3xbarath/slurp)
   - [lazyS3](https://github.com/nahamsec/lazys3)
   - [bucket_finder](https://github.com/msttwidmer/bucket_finder)
   - [ANSBucketDump](https://github.com/netgusto/ANSBucketDump)
   - [sandcastle](https://github.com/DxSearches/sandcastle)
   - [DumpsterDiver](https://github.com/securing/DumpsterDiver)
   - [$3 Bucket Finder](https://github.com/gwen001/s3-buckets-finder)
   - [grayhatwarfare](https://buckets.grayhatwarfare.com)
   - [osint.sh](https://osint.sh/buckets)
   - [s3-detect.yaml](https://github.com/projectdiscovery/nuclei-templates/blob/master/technologies/s3-detect.yaml)

2. **OSINT Tools**:
   - [shodan.io](https://www.shodan.io)
   - https://dnsdumpster.com/
   - [censys.io](https://censys.io)
   - [hunter.io](https://hunter.io)
   - [urlscan.io](https://urlscan.io)
   - [grep.app](https://grep.app)
   - [intelx.io](https://intelx.io)
   - [wigle.net](https://wigle.net)
   - [fullhunt.io](https://fullhunt.io)
   - [vulners.com](https://vulners.com)
   - [viz.greynoise.io](https://viz.greynoise.io)
   - https://hunter.io/search/canon.ie?product_tour_id=389437 (to search about emploeyy emails)

3. **Additional Resources**:
   - [Searx](https://searx.thegpm.org/)
   - [LostSec](https://lostsec.xyz/)
   - [Leak-Lookup](https://leak-lookup.com/account/login)
   - [Dehashed](https://www.dehashed.com/)
   - [Wayback Machine](https://web.archive.org/cdx/search?url=simplisafe.com/&matchType=domain&collapse=urlkey&output=text&fl=original&filter=mimetype:application/x-shockwave-flash&limit=100000&_=1507209148310)
   - [Pentest-Tools](https://pentest-tools.com/information-gathering/google-hacking)
   - [Exploit-DB](https://www.exploit-db.com/google-hacking-database)
   - [Hunter](https://hunter.how/list?searchValue=product.name%3D%22Zabbix%22)
   - [RocketReach](https://rocketreach.co/person?start=1&pageSize=10&link=https://www.linkedin.com/in/brianarsenault)
   - [Lopseg](https://www.lopseg.com.br/dork-helper)
   - [ProxyNova](https://www.proxynova.com/tools/comb/)
   - [Domain Glass](https://domain.glass/)
   - [ZoomEye](https://www.zoomeye.ai/)
   - [Bug Bounty Helper](https://dorks.faisalahmed.me/)
   - [Criminal IP](https://www.criminalip.io/)
   - [vsec7](https://vsec7.github.io/)
   - [Favihash](https://www.favihash.com/)
   - [GooFuzz](https://github.com/m3n0sd0n4ld/GooFuzz)
   - [Awesome-Dorks](https://github.com/0xPugal/Awesome-Dorks)
   - [Shodan-Dorks](https://github.com/humblelad/Shodan-Dorks)
   - [mr-koanti/shodan](https://mr-koanti.github.io/shodan)
   - [Gist](https://gist.github.com)
   - [GitLab](https://gitlab.com)
   - [ContactOut](https://contactout.com)
   - [Crunchbase](https://www.crunchbase.com)
   - [DuckDuckGo](https://duckduckgo.com)
   - [Stack Overflow](https://stackoverflow.com)

4. **Images and Files**:
   - [GaZY2cfXcAAkF3f.jfif](https://prod-files-secure.s3.us-west-2.amazonaws.com/52176e8c-8028-469c-a231-bf6e8894870c/8ccc213a-6eeb-48f8-aec5-b1e9f23cabf1/GaZY2cfXcAAkF3f.jfif)
   - [image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/52176e8c-8028-469c-a231-bf6e8894870c/55e54cd4-48b7-4609-bb59-2988be625987/image.png)

---

5. **Online Websites**
   - [grayhatwarfare](https://buckets.grayhatwarfare.com)
   - [osint.sh](https://osint.sh/buckets)

6. **Nuclei Template**
   - [s3-detect.yaml](https://github.com/projectdiscovery/nuclei-templates/blob/master/technologies/s3-detect.yaml)

7. **Command to Extract $3 Buckets from JS URLs**
   ```bash
   cat js_url.txt | xargs -I {} curl -s {} | grep -oE 'http[s]?://[^/]*\.s3\.amazonaws\.com'
   ```

8. **Using Subfinder and Httpx**
   ```bash
   subfinder -d domain.com -all -silent | httpx -status-code -title -tech-detect | grep "Amazon S3"
   ```

---

### **OSINT Tools and Resources**

- **Shodan**: [shodan.io](https://www.shodan.io)
- **Censys**: [censys.io](https://censys.io) => `https://github.com/Ignitetechnologies/Mindmap/blob/main/Censys/Censys.pdf`
- **Hunter**: [hunter.io](https://hunter.io)
- **Urlscan**: [urlscan.io](https://urlscan.io)
- **Grep.app**: [grep.app](https://grep.app)
- **Intelx**: [intelx.io](https://intelx.io)
- **Wigle**: [wigle.net](https://wigle.net)
- **FullHunt**: [fullhunt.io](https://fullhunt.io)
- **Vulners**: [vulners.com](https://vulners.com)
- **GreyNoise**: [viz.greynoise.io](https://viz.greynoise.io)

---

### **Additional Resources and Dorks**

#### **Search Engines and Tools**
- [Searx](https://searx.thegpm.org/)
- [LostSec](https://lostsec.xyz/)
- [Leak-Lookup](https://leak-lookup.com/account/login)
- [Dehashed](https://www.dehashed.com/)
- [Wayback Machine](https://web.archive.org/cdx/search?url=simplisafe.com/&matchType=domain&collapse=urlkey&output=text&fl=original&filter=mimetype:application/x-shockwave-flash&limit=100000&_=1507209148310)
- [Pentest-Tools](https://pentest-tools.com/information-gathering/google-hacking)
- [Exploit-DB](https://www.exploit-db.com/google-hacking-database)
- [Hunter](https://hunter.how/list?searchValue=product.name%3D%22Zabbix%22)
- [RocketReach](https://rocketreach.co/person?start=1&pageSize=10&link=https://www.linkedin.com/in/brianarsenault)
- [Lopseg](https://www.lopseg.com.br/dork-helper)
- [ProxyNova](https://www.proxynova.com/tools/comb/)
- [Domain Glass](https://domain.glass/)
- [ZoomEye](https://www.zoomeye.ai/)
- [Bug Bounty Helper](https://dorks.faisalahmed.me/)
- [Criminal IP](https://www.criminalip.io/)

---

### **GooFuzz**
```bash
# Install
git clone https://github.com/m3n0sd0n4ld/GooFuzz.git
cd GooFuzz
chmod +x GooFuzz
./GooFuzz -h

# Lists files by extensions separated by commas.
GooFuzz -t nasa.gov -e pdf,bak,old -d 10

# Lists files by extensions contained in a txt file.
GooFuzz -t nasa.gov -e wordlists/extensions.txt -d 30

# List files, directories, and parameters using a wordlist.
GooFuzz -t nasa.gov -w wordlists/words-100.txt -p 3
```

---

### **GitHub Dorks**
```python
".mlab.com password"
"access_key"
"access_token"
"amazonaws"
"api.googlemaps AIza"
"api_key"
"api_secret"
"apidocs"
"apikey"
"apiSecret"
"app_key"
"app_secret"
"appkey"
"appkeysecret"
"application_key"
"appsecret"
"appspot"
"auth"
"auth_token"
"authorizationToken"
"aws_access"
"aws_access_key_id"
"aws_key"
"aws_secret"
"aws_token"
"AWSSecretKey"
"bashrc password"
"bucket_password"
"client_secret"
"cloudfront"
"codecov_token"
"config"
"conn.login"
"connectionstring"
"consumer_key"
"credentials"
"database_password"
"db_password"
"db_username"
"dbpasswd"
"dbpassword"
"dbuser"
"dot-files"
"dotfiles"
"encryption_key"
"fabricApiSecret"
"fb_secret"
"firebase"
"ftp"
"gh_token"
"github_key"
"github_token"
"gitlab"
"gmail_password"
"gmail_username"
"herokuapp"
"internal"
"irc_pass"
"JEKYLL_GITHUB_TOKEN"
"key"
"keyPassword"
"ldap_password"
"ldap_username"
"login"
"mailchimp"
"mailgun"
"master_key"
"mydotfiles"
"mysql"
"node_env"
"npmrc _auth"
"oauth_token"
"pass"
"passwd"
"password"
"passwords"
"pem private"
"preprod"
"private_key"
"prod"
"pwd"
"pwds"
"rds.amazonaws.com password"
"redis_password"
"root_password"
"secret"
"secret.password"
"secret_access_key"
"secret_key"
"secret_token"
"secrets"
"secure"
"security_credentials"
"send.keys"
"send_keys"
"sendkeys"
"SF_USERNAME salesforce"
"sf_username"
"site.com FIREBASE_API_JSON="
"site.com vim_settings.xml"
"slack_api"
"slack_token"
"sql_password"
"ssh"
"ssh2_auth_password"
"sshpass"
"staging"
"stg"
"storePassword"
"stripe"
"swagger"
"testuser"
"token"
"x-api-key"
"xoxb"
"xoxp"
[WFClient] Password= extension:ica
access_key
bucket_password
dbpassword
dbuser
extension:avastlic "support.avast.com"
extension:bat
extension:cfg
extension:env
extension:exs
extension:ini
extension:json api.forecast.io
extension:json googleusercontent client_secret
extension:json mongolab.com
extension:pem
extension:pem private
extension:ppk
extension:ppk private
extension:properties
extension:sh
extension:sls
extension:sql
extension:sql mysql dump
extension:sql mysql dump password
extension:yaml mongolab.com
extension:zsh
filename:.bash_history
filename:.bash_history DOMAIN-NAME
filename:.bash_profile aws
filename:.bashrc mailchimp
filename:.bashrc password
filename:.cshrc
filename:.dockercfg auth
filename:.env DB_USERNAME NOT homestead
filename:.env MAIL_HOST=smtp.gmail.com
filename:.esmtprc password
filename:.ftpconfig
filename:.git-credentials
filename:.history
filename:.htpasswd
filename:.netrc password
filename:.npmrc _auth
filename:.pgpass
filename:.remote-sync.json
filename:.s3cfg
filename:.sh_history
filename:.tugboat NOT _tugboat
filename:_netrc password
filename:apikey
filename:bash
filename:bash_history
filename:bash_profile
filename:bashrc
filename:beanstalkd.yml
filename:CCCam.cfg
filename:composer.json
filename:config
filename:config irc_pass
filename:config.json auths
filename:config.php dbpasswd
filename:configuration.php JConfig password
filename:connections
filename:connections.xml
filename:constants
filename:credentials
filename:credentials aws_access_key_id
filename:cshrc
filename:database
filename:dbeaver-data-sources.xml
filename:deployment-config.json
filename:dhcpd.conf
filename:dockercfg
filename:environment
filename:express.conf
filename:express.conf path:.openshift
filename:filezilla.xml
filename:filezilla.xml Pass
filename:git-credentials
filename:gitconfig
filename:global
filename:history
filename:htpasswd
filename:hub oauth_token
filename:id_dsa
filename:id_rsa
filename:id_rsa or filename:id_dsa
filename:idea14.key
filename:known_hosts
filename:logins.json
filename:makefile
filename:master.key path:config
filename:netrc
filename:npmrc
filename:pass
filename:passwd path:etc
filename:pgpass
filename:prod.exs
filename:prod.exs NOT prod.secret.exs
filename:prod.secret.exs
filename:proftpdpasswd
filename:recentservers.xml
filename:recentservers.xml Pass
filename:robomongo.json
filename:s3cfg
filename:secrets.yml password
filename:server.cfg
filename:server.cfg rcon password
filename:settings
filename:settings.py SECRET_KEY
filename:sftp-config.json
filename:sftp-config.json password
filename:sftp.json path:.vscode
filename:shadow
filename:shadow path:etc
filename:spec
filename:sshd_config
filename:token
filename:tugboat
filename:ventrilo_srv.ini
filename:WebServers.xml
filename:wp-config
filename:wp-config.php
filename:zhrc
HEROKU_API_KEY language:json
HEROKU_API_KEY language:shell
HOMEBREW_GITHUB_API_TOKEN language:shell
jsforce extension:js conn.login
language:yaml -filename:travis
msg nickserv identify filename:config
org:Target "AWS_ACCESS_KEY_ID"
org:Target "list_aws_accounts"
org:Target "aws_access_key"
org:Target "aws_secret_key"
org:Target "bucket_name"
org:Target "S3_ACCESS_KEY_ID"
org:Target "S3_BUCKET"
org:Target "S3_ENDPOINT"
org:Target "S3_SECRET_ACCESS_KEY"
password
path:sites databases password
private -language:java
PT_TOKEN language:bash
redis_password
root_password
secret_access_key
SECRET_KEY_BASE=
shodan_api_key language:python
WORDPRESS_DB_PASSWORD=
xoxp OR xoxb OR xoxa
s3.yml
.exs
beanstalkd.yml
deploy.rake
.sls
AWS_SECRET_ACCESS_KEY
API KEY
API SECRET
API TOKEN
ROOT PASSWORD
ADMIN PASSWORD
GCP SECRET
AWS SECRET
"private" extension:pgp
```

---

### **Shodan Dorks**
```bash
ssl.cert.subject.cn:"att.com" -http.title:"404"
ssl:apple
org:Apple Inc*
ssl:apple.com
```

---

### **Hash Dorking**
- [Favihash](https://www.favihash.com/)

---

### **FOFA Dorks**
```bash
site:target.com intext:"sql syntax near" | intext:"syntax error has occurred" | intext:"incorrect syntax near" | intext:"unexpected end of SQL command" | intext:"Warning: mysql_connect()" | intext:"Warning: mysql_query()" | intext:"Warning: pg_connect()"
```

---

### **Other Platforms**
- [Gist](https://gist.github.com)
- [GitLab](https://gitlab.com)
- [ContactOut](https://contactout.com)
- [Crunchbase](https://www.crunchbase.com)
- [DuckDuckGo](https://duckduckgo.com)
- [Stack Overflow](https://stackoverflow.com)

---

### **Dorking Sites**

### **Slack Invites**
```bash
site:http://join.slack.com
site:http://docs.google.com "company name"
site:http://groups.google.com "company name"
```

### **Cloud Storage**
```bash
site:http://s3.amazonaws.com "target.com"
site:http://blob.core.windows.net "target.com"
site:http://googleapis.com "target.com"
site:http://drive.google.com "target.com"
```

### **URL Scanning**
```bash
https://otx.alienvault.com/api/v1/indicators/hostname/site.com/url_list?limit=500&page=1
https://web.archive.org/cdx/search/cdx?url=*.join.slack.com&fl=original&collapse=urlkey
```
- https://www.exploit-db.com/google-hacking-database

--- 

## **Esoteric Techniques in Reconnaissance**  

### **Why?**  
- Discover **hidden assets** that don’t appear in regular scans.  
- Uncover **relationships between domains, servers, and services**.  
- Exploit **forgotten or misconfigured data** to gain intelligence.  
- Find **sensitive information** that traditional OSINT techniques might miss.  

### **1️⃣ Tracking Codes & Ad Networks**  
Extract tracking IDs (Google Analytics, Facebook Pixel, AdSense) to find related domains.  
```sh
curl -s https://example.com | grep -oE "UA-[0-9]+-[0-9]+"
```
Search for related domains using:  
- 🔗 [BuiltWith](https://builtwith.com/)  
- 🔗 [SpyOnWeb](https://spyonweb.com/)  

### **2️⃣ Cache Snooping – Extracting Old Versions of Pages**  
Use public caches to retrieve deleted or modified content.  
```sh
https://webcache.googleusercontent.com/search?q=cache:example.com
```
Retrieve historical URLs:  
```sh
echo "example.com" | waybackurls
```

### **3️⃣ Bypassing CDN Protection to Reveal Real IP**  
Extract SPF records to find mail servers that might expose the real IP.  
```sh
dig TXT example.com +short | grep "spf"
```
Attempt direct requests to potential real IPs:  
```sh
curl -H "Host: example.com" --resolve example.com:80:[IP] http://example.com
```

### **4️⃣ Analyzing Public Repositories for Leaked Data**  
Search GitHub for exposed credentials, API keys, or sensitive data.  
```sh
github.com/search?q="example.com" type=code
```
Scan repositories for secrets:  
```sh
gitleaks --repo=https://github.com/example/repo.git
```

### **5️⃣ Extracting Metadata from Leaked Documents**  
Find sensitive user and system information hidden in document metadata.  
```sh
exiftool leaked_document.pdf
```
Search Google for exposed documents:  
```google
site:example.com filetype:pdf OR filetype:docx OR filetype:xls
```

### **Key Takeaways:**  
✅ **Analyze tracking codes** to find linked domains.  
✅ **Leverage cache archives** to recover deleted data.  
✅ **Bypass CDN protection** to reveal real backend servers.  
✅ **Mine public repositories** for exposed secrets.  
✅ **Extract metadata** from leaked documents to uncover user and system info.  


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
  naabu -list sub-list.txt -top-ports 1000 -exclude-ports 80,443,21,22,25 -o ports.txt
  naabu -list sub-list.txt -p - -exclude-ports 80,443,21,22,25  -o ports.txt
  ```
- **Nmap**:
  ```bash
  nmap -sV -sC -sS 87.251.41.115

  nmap 127.0.0.1 -sS -sV -Pn -n — max-rate 1000 — open -p 21 -oN Active_21.txt

  Tip: Don’t forgot to scan the commonly open ports like 21,22,3000,8080,8000,8081,8008,8888,8443,9000,9001,9090.
  ```
- **httprobe**:
  ```bash
  cat domains | httprobe -c 80 -h 
  ```


--- 



## 16. Claim URLs

### **Web Crawling and URL Discovery Tools**

#### **1. Katana**
- **GitHub Repository**: [Katana by ProjectDiscovery](https://github.com/projectdiscovery/katana)
- **Installation**:
  ```bash
  go install github.com/projectdiscovery/katana/cmd/katana@latest
  ```
- **Basic Usage**:
  ```bash
  katana -u https://tesla.com
  ```
- **List Input**:
  ```bash
  cat url_list.txt | katana -list
  ```
- **More Info**: [Katana GitHub Page](https://github.com/projectdiscovery/katana)

---

#### **2. GoSpider**
- **GitHub Repository**: [GoSpider by Jaeles Project](https://github.com/jaeles-project/gospider)
- **Installation**:
  ```bash
  go get -u github.com/jaeles-project/gospider
  ```
- **Basic Usage**:
  ```bash
  gospider -s "https://google.com/" -o output -c 10 -d 1
  ```
- **Advanced Usage**:
  - Run with site list:
    ```bash
    gospider -S sites.txt -o output -c 10 -d 1
    ```
  - Include 3rd party sources (Archive.org, CommonCrawl.org, VirusTotal.com, AlienVault.com) and subdomains:
    ```bash
    gospider -s "https://google.com/" -o output -c 10 -d 1 --other-source --include-subs
    ```
  - Blacklist URL/file extensions:
    ```bash
    gospider -s "https://google.com/" -o output -c 10 -d 1 --blacklist ".(woff|pdf)"
    ```

---

#### **3. GetAllUrls (GAU)**
- **GitHub Repository**: [GAU by lc](https://github.com/lc/gau)
- **Installation**:
  ```bash
  go get -u -v github.com/lc/gau
  ```
- **Usage**:
  - Extract subdomains:
    ```bash
    gau -subs example.com | cut -d / -f 3 | sort -u
    ```
  - Extract URLs excluding specific file types:
    ```bash
    cat sub.txt | gau -b png,jpg,gif,jpeg,swf,woff,gif,svg -o allurls.txt
    ```

---

#### **4. URLFinder**
- **GitHub Repository**: [URLFinder ](https://github.com/projectdiscovery/urlfinder)
- **Installation**:
  ```bash
  go install -v github.com/projectdiscovery/urlfinder/cmd/urlfinder@latest
  ```
- **Usage**:
  - Extract subdomains:
    ```bash
    urlfinder -h
    ```
  - Basic Usage:
    ```bash
    urlfinder -d tesla.com
    ```
---


---

- **Waymore**:
  ```bash
  waymore -i $domain -mode U -oU ./waymoreUrls.txt -url-filename -p 4
  -- X Tip's to se some endpoint't in non 80,443 Port's
  waymore -i http://domain.com -mode U -oU waymore_output.txt
  cat waymore_output.txt | grep "com:" | grep -v ":80" | grep -v ":443"
  --
  ```
- **Gauplus and Hakrawler**:
  ```bash
  echo $domain | (gauplus || hakrawler) | grep -Ev "\.(jpeg|jpg|png|ico|woff|svg|css|ico|woff|ttf)$" > ./gaukrawler.txt
  ```

#### **3. Hakrawler**
- **Purpose**: A web crawler for discovering URLs and endpoints.
- **Repo** : https://github.com/hakluke/hakrawler
- **Installation**:
  ```bash
  go install github.com/hakluke/hakrawler@latest
  ```
- **Usage**:
  - Single URL:
    ```bash
    echo https://google.com | hakrawler
    ```
  - Multiple URLs:
    ```bash
    cat urls.txt | hakrawler
    ```
---
  
  

### **Web Archiving and Historical Data**

#### **1. Wayback Machine (Archive.org)**
- **Usage**:
  ```bash
  https://web.archive.org/cdx/search/cdx?url=*.redacted.com%2F*&output=text&fl=original&collapse=urlkey&filter=statuscode%3A200
  ```
- **Example Commands**:
  ```bash
  echo "example.com" | waybackurls | grep -iE '\.js' | grep -ivE '\.json' | sort -u > j.txt
  echo "example.com" | waybackurls | httpx > live.txt
  ```

---

#### **2. Archive.ph**
- **Website**: [Archive.ph](https://archive.ph/)
- **Purpose**: A tool for saving and accessing archived web pages.

---

### **URL Scanning and Analysis Tools**

#### **1. URLScan.io**
- **Website**: [URLScan.io](https://urlscan.io/)
- **Search Functionality**: [URLScan.io Search](https://urlscan.io/search#target.com)
- **Purpose**: Scan and analyze URLs and websites.

---

#### **2. Fofa.info**
- **Website**: [Fofa.info](https://en.fofa.info)
- **Purpose**: A search engine for internet-connected devices and services.

---

### **JavaScript Tools**

#### **1. JavaScript Code for Scanning**
- **Code**:
  ```javascript
  javascript:(async function(){let scanningDiv=document.createElement("div");scanningDiv.style.position="fixed",scanningDiv.style.bottom="0",scanningDiv.style.left="0",scanningDiv.style.width="100%",scanningDiv.style.maxHeight="50%",scanningDiv.style.overflowY="scroll",scanningDiv.style.backgroundColor="white",scanningDiv.style.color="black",scanningDiv.style.padding="10px",scanningDiv.style.zIndex="9999",scanningDiv.style.borderTop="2px solid black",scanningDiv.innerHTML="<h4>Scanning...</h4>",document.body.appendChild(scanningDiv);let e=[],t=new Set;async function n(e){try{const t=await fetch(e);return t.ok?await t.text():(console.error(`Failed to fetch ${e}: ${t.status}`),null)}catch(t){return console.error(`Error fetching ${e}:`,t),null}}function o(e){return(e.startsWith("/")||e.startsWith("./")||e.startsWith("../"))&&!e.includes(" ")&&!/[^\x20-\x7E]/.test(e)&&e.length>1&&e.length<200}function s(e){return[...e.matchAll(/['"]((?:\/|\.\.\/|\.\/)[^'"]+)['"]/g)].map(e=>e[1]).filter(o)}async function c(o){if(t.has(o))return;t.add(o),console.log(`Fetching and processing: ${o}`);const c=await n(o);if(c){const t=s(c);e.push(...t)}}const l=performance.getEntriesByType("resource").map(e=>e.name);console.log("Resources found:",l);for(const e of l)await c(e);const i=[...new Set(e)];console.log("Final list of unique paths:",i),console.log("All scanned resources:",Array.from(t)),scanningDiv.innerHTML=`<h4>Unique Paths Found:</h4><ul>${i.map(e=>`<li>${e}</li>`).join("")}</ul>`})();
  ```
- **Purpose**: A JavaScript snippet for scanning and extracting unique paths from a webpage.

---



### **Other Tools and Resources**

#### **1. AlienVault**
- **Purpose**: A threat intelligence platform for detecting and analyzing threats.

#### **2. VirusTotal**
- **Purpose**: A service for analyzing files and URLs for malware.



---

### **Combined Workflow Example**

```bash
cat live | tee >(gau --fp | sort | uniq | cat way | grep -Ev '\.(png|jpg|gif|jpeg|swf|woff|svg)$' > way1 >(waybackurls | sort | uniq cat way | grep -Ev '\.(png|jpg|gif|jpeg|swf|woff|svg)$' > way1 | sort | uniq > combined_urls.txt && cat combined_urls.txt | httpx > urls && cat urls | grep "=" > params && cat urls | grep ".js" > js
```

---

### **Additional Notes**


- **Combining Results**:
  ```bash
  cat ./waymoreUrls.txt ./gaukrawler.txt | sort -u | uro | gf endpoints > allUrls.txt
  ```

--- 


## 17. Scan JS-Files

### 1. **Check JS File Status Code**
   - **Tool**: `hakcheckurl`
   - **Repository**: [hakluke/hakcheckurl](https://github.com/hakluke/hakcheckurl)
   - **Installation**:
     ```bash
     go get github.com/hakluke/hakcheckurl
     ```
   - **Usage**:
     ```bash
     cat lyftgalactic-js-urls.txt | hakcheckurl
     ```

---

### 2. **Extract Endpoints from JS Files**
   - **Tool**: `LinkFinder`
   - **Repository**: [GerbenJavado/LinkFinder](https://github.com/GerbenJavado/LinkFinder)
   - **Installation**:
     ```bash
     git clone https://github.com/GerbenJavado/LinkFinder.git
     cd LinkFinder
     python setup.py install
     ```
   - **Tool**: `relative-url-extractor`
   - **Repository**: [jobertabma/relative-url-extractor](https://github.com/jobertabma/relative-url-extractor)
   - **Usage**:
     ```bash
     ruby extract.rb https://www.domain.com/jspath/code.js
     ```
   -  **CWFF**
  - **GitHub Repository**: [CWFF by D4Vinci](https://github.com/D4Vinci/CWFF)
  - **Installation**: // this tool to claim .js files then extract endpoint's from it
    ```bash
    git clone https://github.com/D4Vinci/CWFF
    cd CWFF
    python3 -m pip install -r requirements.txt
    python3 cwff.py --help
    ```
  - **Usage**:
  
    - Basic Usage:
      ```bash
      CWFF [-h] [--threads] [--github] [--subdomains] [--recursive] [--js-libraries] [--connected-websites] [--juicy-files] [--use-filter-model] [- 
      o] 
      domain
  
      positional arguments:
        domain                Target website(ofc)
      
      optional arguments:
        -h, --help            Show this help message and exit
        --threads             The number of maximum concurrent threads to use (Default:1000)
        --github              Collect endpoints from a given github repo (ex:https://github.com/google/flax)
        --subdomains          Extract endpoints from subdomains also while search in the wayback machine!
        --recursive           Work on extracted endpoints recursively (Adds more endpoints but less accurate sometimes)!
        --js-libraries        Extract endpoints from JS libraries also, not just the JS written by them!
        --connected-websites  Include endpoints extracted from connected websites
        --juicy-files         Include endpoints extracted from juicy files like sitemap.xml and robots.txt
        --use-filter-model    Filter result endpoints with filter_model file
        -o                    The output directory for the endpoints and parameters. (Default: website name)
        ```
  

---

### 3. **Scan for Secrets and Passwords**
   - **Tool**: `DumpsterDiver`
   - **Repository**: [securing/DumpsterDiver](https://github.com/securing/DumpsterDiver)
   - **Usage**:
     ```bash
     python3 DumpsterDiver.py
     ```

---

### 4. **Filter JS Files from a List of URLs**
   - **Command**:
     ```bash
     cat allurls.txt | grep "\.js" > js-files.txt
     ```

---

### 5. **Regex for Detecting Secrets in JS Files**
   - **Tool**: `jsecret`
   - **Repository**: [raoufmaklouf/jsecret](https://github.com/raoufmaklouf/jsecret)
   - **Regex Pattern**:
     ```regex
     (?i)(api[_-]?key|secret[_-]?key|access[_-]?token|auth[_-]?token|client[_-]?id|client[_-]?secret|private[_-]?key|consumer[_-]?key|bearer[_-]?token|aws[_-]?secret|aws[_-]?key|github[_-]?token|admin(?:istrator)?[_-]?(?:key|password|token)|superuser[_-]?password|root[_-]?password)\s*[:=]\s*["']?[A-Za-z0-9._-]{8,}["']?
     ```

---

### Summary of Tools and Commands:
1. **Check JS File Status Code**: Use `hakcheckurl` to verify the status of JS files.
2. **Extract Endpoints**: Use `LinkFinder` or `relative-url-extractor` to extract endpoints from JS files.
3. **Scan for Secrets**: Use `DumpsterDiver` to scan for secrets and passwords.
4. **Filter JS Files**: Use `grep` to filter JS files from a list of URLs.
5. **Detect Secrets with Regex**: Use the provided regex pattern to detect secrets in JS files.

--- 
---


## 18. Hidden Parameters

### **Parameter Fuzzing Tools and Techniques**

#### **1. FFUF (Fuzz Faster U Fool)**
- **Installation:**
  ```bash
  git clone https://github.com/ffuf/ffuf
  cd ffuf
  go get
  go build
  ```

- **GET Parameter Fuzzing:**
  - Fuzz parameter names:
    ```bash
    ffuf -w /path/to/paramnames.txt -u https://target/script.php?FUZZ=test_value -fs <Number of Default Length>
    ```
    *Assumes a response size of 4242 bytes for invalid GET parameter names.*

  - Fuzz parameter values (if the parameter name is known):
    ```bash
    ffuf -w /path/to/values.txt -u https://target/script.php?valid_name=FUZZ -fc 401
    ```
    *Assumes a wrong parameter value returns HTTP response code 401.*

- **POST Data Fuzzing:**
  ```bash
  ffuf -w /path/to/postdata.txt -X POST -d "username=admin&password=FUZZ" -u https://target/login.php -fc 401
  ```
  *Fuzzes part of the POST request and filters out 401 responses.*

- **Advanced FFUF Examples:**
  ```bash
  ffuf -w params.txt -u http://ffuf.me/cd/param/data?FUZZ=1 -c true -s -t 99 -rate 660
  ffuf -request req.txt -w params.txt -c true -s -t 99 -rate 660 [in request param=FUZZ]
  ffuf -w params.txt -u http://ffuf.me/cd/param/data? -c true -s -t 99 -rate 660 -X POST -d '{"name": "FUZZ", "anotherkey": "anothervalue"}'
  ```

- **Learn More About FFUF:**
  [FFUF GitHub Repository](https://github.com/ffuf/ffuf)

---

#### **2. X8**
- **GitHub Repository:**
  [X8 by Sh1Yo](https://github.com/Sh1Yo/x8)

---

#### **3. InputScanner**
- **GitHub Repository:**
  [InputScanner by zseano](https://github.com/zseano/InputScanner)

---

#### **4. LinkFinder**
- **GitHub Repository:**
  [LinkFinder by GerbenJavado](https://github.com/GerbenJavado/LinkFinder)

---

#### **5. Parameth**
- **GitHub Repository:**
  [Parameth by maK-](https://github.com/maK-/parameth)

---

#### **6. Arjun**
- **Installation:**
  ```bash
  pip3 install arjun
  ```

- **Usage:**
  - Scan a single URL:
    ```bash
    arjun -u https://api.example.com/endpoint
    ```
  - Import targets from a file:
    ```bash
    arjun -i targets.txt
    ```
  - Advanced GET parameter discovery:
    ```bash
    arjun -i urls.txt -t 90 -oT getparams.txt -w paramswordlist.txt -m GET --stable --disable-redirects --headers "Accept-Language: en-US\nCookie: null"
    ```
  - Advanced POST parameter discovery:
    ```bash
    arjun -i urls.txt -t 90 -oT postparams.txt -w paramswordlist.txt -m POST --stable --disable-redirects --headers "Accept-Language: en-US\nCookie: null"
    ```
  - API (REST) parameter discovery:
    ```bash
    arjun -i urls.txt -t 90 -oT jsonparams.txt -w paramswordlist.txt -m JSON --stable --disable-redirects --headers "Accept-Language: en-US\nCookie: null"
    ```
  - API (SOAP) parameter discovery:
    ```bash
    arjun -i urls.txt -t 90 -oT soapparams.txt -w paramswordlist.txt -m XML --stable --disable-redirects --headers "Accept-Language: en-US\nCookie: null"
    ```

---

#### **7. ParamPamPam**
- **Installation:**
  ```bash
  git clone https://github.com/Bo0oM/ParamPamPam.git
  cd ParamPamPam
  pip3 install --no-cache-dir -r requirements.txt
  ```

- **Usage:**
  - GET parameter discovery:
    ```bash
    python3 parampp.py -u "https://vk.com/login" -m GET -f getparamsout.txt
    ```
  - POST parameter discovery:
    ```bash
    python3 parampp.py -u "https://vk.com/login" -m POST -f postparamsout.txt
    ```

---

#### **8. ParamSpider**
- **GET Parameter Fuzzing:**
  ```bash
  for URL in $(<php_endpoints_urls.txt); do (ffuf -u "${URL}?FUZZ=1" -w params_list.txt -mc 200 -ac -sa -t 20 -or -od ffuf_hidden_params_sqli_injections); done
  ```

- **POST Parameter Fuzzing:**
  ```bash
  for URL in $(<php_endpoints_urls.txt); do (ffuf -X POST -u "${URL}" -w params_list.txt -mc 200 -ac -sa -t 20 -or -od ffuf_hidden_params_sqli_injections -d "FUZZ=1"); done
  ```

--- 


## 19. Employee Enumeration
- **GitHub**:
  Use GitHub dorks to find employee information.

## 20. S3 Bucket Enumeration
- **S3Scanner**:
  ```bash
  s3scanner -l domains.txt
  ```

--- 


## 21. Subdomain Monitoring
- **Facebook CT Search**:
  [Facebook CT Search](https://developers.facebook.com/tools/ct/search/)

--- 


## 22. Nuclei
- **Nuclei**:
  ```bash
  nuclei -l -t ~/nuclei-templates/http/exposures/
  ```

--- 


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
- https://www.offensity.com/en/blog/just-another-recon-guide-pentesters-and-bug-bounty-hunters/
- # recon-resource
- https://blog.projectdiscovery.io/reconnaissance-series-5-additional-active-reconnaissance/
- https://twitter.com/ReconOne_bk/status/1642431028470874116?t=NSn9iile3U4Il4UiV9ai5Q&s=08
- https://www.offensity.com/en/blog/just-another-recon-guide-pentesters-and-bug-bounty-hunters/?ref=blog.projectdiscovery.io
- https://bugbountyforum.com/tools/recon/
- https://thecyberblogs.com/the-bug-hunters-methodology-v4-recon-tbhmv4/
- https://eslam3kl.gitbook.io/blog/recon-automation-tips-and-tricks/simple-recon-methodology
- https://shahjerry33.medium.com/github-recon-its-really-deep-6553d6dfbb1f
- https://orwaatyat.medium.com/
- https://web.archive.org/web/20221209143311/https://twitter.com/GodfatherOrwa
- https://doepichack.com/category/tips/
- https://blog.intigriti.com/2023/01/18/bug-bytes-190-bbtips-attacking-wide-scopes-aws-and-containers/?utm_source=newsletter&utm_medium=email&utm_campaign=bug_bytes_190_bbtips_attacking_wide_scopes_aws_and_containers&utm_term=2023-01-18
- https://portswigger.net/research/top-10-web-hacking-techniques-of-2022-nominations-open
- https://m0chan.github.io/2019/12/17/Bug-Bounty-Cheetsheet.html
- https://eforensicsmag.com/bug-bounty-methodology-ttp-tacticstechniques-and-procedures-v-2-0/
- [How To Attack Admin Panels Successfully Part 2](https://medium.com/geekculture/how-to-attack-admin-panels-successfully-part-2-9316c3caad3a)
-   [Open redirects : bug bounties](https://falkensmaze.medium.com/open-redirects-bug-bounties-28efffb635c?source=rss------bug_bounty-5)
-   [Seven Common Ways To Bypass Login Page](https://medium.com/@uttamgupta_/seven-common-ways-to-bypass-login-page-a023d9dd073c?source=rss------bug_bounty-5)
-   [Unlock the boundless possibilities of ChatGPT: Hunt down pesky bugs and enjoy seamless automation!](https://hunkyhack3r.medium.com/unlock-the-boundless-possibilities-of-chatgpt-hunt-down-pesky-bugs-and-enjoy-seamless-automation-21ecfdf472cd?source=rss------bug_bounty-5)
-   [Broken Access Control: What I have learned](https://juliosergiofs.medium.com/broken-access-control-what-i-have-learned-1223874179cd?source=rss------bug_bounty-5)
-   [Bug Hunting 101: Parameter Injection Vulnerabilities](https://grahamzemel.medium.com/bug-hunting-101-parameter-injection-vulnerabilities-bedab284b4d9?source=rss------bug_bounty-5)
-   [JWT Security 101: How to defend against common attacks on JSON Web Tokens](https://medium.com/@mk2011sharma/jwt-security-101-how-to-defend-against-common-attacks-on-json-web-tokens-632fa09d8887?source=rss------bug_bounty-5)
-  https://medium.com/@TheUnwearyPentester/brute-force-attacks-cheat-sheet-ftp-pop3-snmp-ssh-vnc-f8f37cfe4ec?source=rss------bug_bounty-5
-   https://infosecwriteups.com/clear-communication-is-crucial-why-writing-effective-vulnerability-reports-matters-5f989ee2e401?source=rss----7b722bfd1b8d---4
-   https://sl4x0.medium.com/all-about-business-logic-bugs-803fa0df9eb4?source=rss------bug_bounty-5
-  https://adamjsturge.medium.com/easy-xsshunter-express-setup-script-d5a66039f7b6?source=rss------bug_bounty-5
-   https://thegrayarea.tech/bug-hunting-101-directory-enumeration-authentication-bypass-1b92b3c87ef9?source=rss------bug_bounty-5
-  https://infosecwriteups.com/kerberos-authentication-again-but-better-badb5dc88b2d?source=rss----7b722bfd1b8d---4
-   https://infosecwriteups.com/bypass-mysql-real-escape-string-and-addslashes-from-injection-attacks-6e64508e011b?source=rss----7b722bfd1b8d---4
-   https://infosecwriteups.com/domain-name-system-0x1-dns-101-cb0aba088abb?source=rss----7b722bfd1b8d---4
-  https://infosecwriteups.com/the-toddlers-introduction-to-dynamic-memory-allocation-300f312cd2db?source=rss----7b722bfd1b8d---4
-   https://web3datadegens.substack.com/p/2023-guide-to-web3-data-tools
-   https://jakewnuk.com/posts/optimizing-wordlists-w-masks/
-   https://infosecwriteups.com/how-i-found-aws-api-keys-using-trufflehog-and-validated-them-using-enumerate-iam-tool-cd6ba7c86d09?source=rss----7b722bfd1b8d---4
- https://rashahacks.com/how-i-pwned-10-admin-panels-and-rewarded-8000/
- https://pentester.land/blog/subdomains-enumeration-cheatsheet/
- https://github.com/supr4s/VPS-web-hacking-tools
- https://github.com/0xJin/awesome-bugbounty-builder
- https://reconshell.com/bug-bounty-tips/
- https://reconshell.com/bug-bounty-resources/
- https://0xffsec.com/handbook/information-gathering/subdomain-enumeration/
- https://lazyhacker.medium.com/subdomain-enumeration-tec-276da39d7e69
- https://reconshell.com/web-attack-cheat-sheet/
- https://techvomit.net/web-application-penetration-testing-notes/
- https://edoverflow.com/2017/lightweight-reconnaissance-setup/
- https://blog.securitybreached.org/2017/11/25/guide-to-basic-recon-for-bugbounty/
- https://github.com/zapstiko/Bug-Bounty
- https://highon.coffee/blog/password-reset-security-testing-cheat-sheet/
- https://github.com/Mostafa-Elguerdawi/Automation_Bug_Hunting/tree/main/Automation
- https://www.infosecmatter.com/bug-bounty-tips/
- https://inonst.medium.com/31-tips-advanced-bug-bounty-pentesting-8ecec2aafffe
- https://cheatsheet.haax.fr/web-pentest/bug_bounty_tips/
- https://www.pinterest.com/thehackerish/bug-bounty/
- https://www.offensity.com/en/blog/just-another-recon-guide-pentesters-and-bug-bounty-hunters/
- https://github.com/topics/bugbountytips
- https://mavericknerd.github.io/knowledgebase/BugBountyRecon/
- https://shubs.io/so-you-want-to-get-into-bug-bounties/
- https://rattibha.com/thread/1590657312280436738?lang=ar
- https://www.offensity.com/en/blog/just-another-recon-guide-pentesters-and-bug-bounty-hunters/
-   [A few dorks to find common bugs while testing](https://twitter.com/ADITYASHENDE17/status/1613423367448821762?s=20)
-   [OWA tip](https://twitter.com/irsdl/status/1613702363587436547?s=20)
-   [Top 10 web hacking techniques of 2022 voting](https://portswigger.net/polls/top-10-web-hacking-techniques-2022)
-   [TodayIsNew Interview with his tips](https://twitter.com/Bugcrowd/status/1612874185465892865)
-   [Recon management tips by Jason Haddix](https://twitter.com/Jhaddix/status/1615453280015982593?s=20)
-   [PHP info page pays out $5k](https://twitter.com/SaraBadran18/status/1615347898392117249?s=20)
-   [Custom wordlists tip](https://twitter.com/dsopas/status/1613886178524106756?s=20)
-   [SQL injection payloads](https://twitter.com/nav1n0x/status/1613225462154596375?s=20)
- https://reconshell.com/bug-bounty-tips/
- https://blog.yeswehack.com/yeswerhackers/parameter-discovery-quick-guide-to-start/
- https://yogosha.com/blog/bug-bounty-practical-guide-for-organizations/
- https://www.cyberick.com/post/recon-automation-tips-bug-bounty
- https://www.bugbountyclub.com/tips
- https://infosecsanyam.medium.com/web-application-security-bug-bounty-methodology-reconnaissance-vulnerabilities-reporting-635073cddcf2
- https://agent472458.medium.com/my-recon-tools-and-methodology-17abf8080f3c
- https://infosecwriteups.com/recon-everything-48aafbb8987
- https://infosecwriteups.com/bug-hunting-journey-of-2021-1fa60b28d949
- https://apexvicky.medium.com/bug-bounty-methodology-horizontal-enumeration-89f7cd172e6e
- https://medium.com/@ehsahil/recon-my-way-82b7e5f62e21
- https://fyrmassociates.com/blog/2019/03/01/advanced-recon-part1/
- https://www.bugbountyhunter.com/articles/?on=Advanced_Recon
- https://m0chan.github.io/2019/12/17/Bug-Bounty-Cheetsheet.html
- https://community.turgensec.com/shodan-pentesting-guide/
- https://seqred.pl/en/shodan-improved/
- https://docs.servicenow.com/pt-BR/bundle/sandiego-security-management/page/product/secops-integration-vr/shodan/concept/shodan-exploit-vuln-integration.html
- https://pentester.land/blog/levelup-2018-the-bug-hunters-methodology-v3/
- https://0x00sec.org/t/using-search-engines-for-fun-and-bounties/23832
- https://null-byte.wonderhowto.com/how-to/hack-like-pro-find-vulnerable-targets-using-shodan-the-worlds-most-dangerous-search-engine-0154576/
- https://securitytrails.com/blog/hacker-search-engines
- https://null-byte.wonderhowto.com/how-to/hack-like-pro-reconnaissance-with-recon-ng-part-1-getting-started-0169854/
- https://infosecwriteups.com/guide-to-basic-recon-bug-bounties-recon-728c5242a115
- https://blog.usejournal.com/how-recon-helped-samsung-protect-their-production-repositories-of-samsungtv-ecommerce-estores-4c51d6ec4fdd
- https://blog.zsec.uk/ltr101-method-to-madness/
- https://github.com/bugcrowd/bugcrowd_university/blob/master/Recon%20and%20Discovery/Bugcrowd%20University%20-%20Recon%20%26%20Discovery.pdf
- https://medium.com/@maverickNerd/recon-everything-48aafbb8987
- https://mavericknerd.github.io/knowledgebase/
- https://blog.detectify.com/2020/01/07/guest-blog-streaak-my-recon-techniques-from-2019/
- https://medium.com/@europa_/recoinnassance-7840824b9ef2
- https://medium.com/@ehsahil/recon-my-way-82b7e5f62e21
- https://medium.com/bugbountywriteup/whats-tools-i-use-for-my-recon-during-bugbounty-ec25f7f12e6d
- https://bugbountytuts.files.wordpress.com/2018/02/dirty-recon.pdf
- https://blog.usejournal.com/web-application-security-bug-bounty-methodology-reconnaissance-vulnerabilities-reporting-635073cddcf2
- http://imsoley.tk/mofb/
- https://blog.detectify.com/2019/01/29/hacking-isnt-an-exact-science/
- https://docs.google.com/presentation/d/1xgvEScGZ_ukNY0rmfKz1JN0sn-CgZY_rTp2B_SZvijk/edit#slide=id.g3fc0937313_1_68
- https://sylarsec.com/2019/01/11/100-ways-to-discover-part-1/
- https://captmeelo.com/bugbounty/2019/09/02/asset-enumeration.html
- https://medium.com/@shahjerry33/recon-my-way-or-high-way-58a18dab5c95
- https://medium.com/@smhtahsin33/recon-one-step-advanced-with-otx-8827119566fd
- https://eslam3kl.medium.com/simple-recon-methodology-920f5c5936d4
- https://systemweakness.com/ultimate-manual-bug-bounty-recon-guide-f30c900367c8

