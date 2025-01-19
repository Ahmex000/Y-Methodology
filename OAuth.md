---

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
