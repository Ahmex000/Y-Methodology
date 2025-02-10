---

### **Email Injection & Bypass Techniques**

#### **1. Email Injection Patterns**
- email1/=email2
- {email1;...;email2}
- email1f=$email2
- [email1,email2]
- $(email1:email2)
- email1 >> email2;
- [email1]#[email2]
- email1==email2
- email1*1=email2*1
- email1^email2
- email1;email2
- email1->email2
- email1 ... email2
- email1 != email2
- email1'...'email2
- email1/../../email2
- (email1)=email2
- /email1/+/email2/
- email1@email2@
- email1;email2
- email1×0=1=email2
- email1|email2
- email1(*)email2
- email1#>email2
- \email1\email2
- \email1/email2

#### **2. Email Bypass Examples**
- email@email.com,victim@hack.secry
- email@email“,”victim@hack.secry
- email@email.com:victim@hack.secry
- email@email.com%0d%0avictim@hack.secry
- %0d%0avictim@hack.secry
- %0avictim@hack.secry
- victim@hack.secry%0d%0a
- victim@hack.secry%0a
- victim@hack.secry%0d
- victim@hack.secry%00
- victim@hack.secry{{}}

---

### **Registration & Signup Vulnerabilities**

#### **1. Duplicate Registration / Overwrite Existing User**
- Create an account with the same email but different password.
- Check if the system allows overwriting existing users.

#### **2. Cross-Site Scripting (XSS) in Username or Account Name**
- Test for XSS in username, account name, or other registration fields.

#### **3. Insufficient Email Verification**
- Bypass email verification using techniques like:
  - Response manipulation.
  - SQL injection in verification process.
  - Client-side validation bypass.

#### **4. Weak Password Policy**
- Check if the application allows weak passwords like "123456" or "password".
- Test if the password can be the same as the email address.

#### **5. Weak Registration Implementation**
- Test for vulnerabilities in the registration process, such as:
  - Lack of input validation.
  - Insecure storage of user data.
  - Missing rate limiting.

---

### **ORM Parameter Manipulation**
- Example: `user[admin]=true`
- Exploit ORM (Object-Relational Mapping) by manipulating object parameters.

---

### **References**
- Test Cases for Registration Page: https://katalon.com/resources-center/blog/test-cases-for-registration-page
- OWASP Testing Guide - User Registration: https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/03-Identity_Management_Testing/02-Test_User_Registration_Process
- HackTricks - Registration Vulnerabilities: https://hacktricks.boitatech.com.br/pentesting-web/registration-vulnerabilities

---

### **Steps for Testing**
1. **Account Creation**:
   - Create an account with a valid email and password.
   - Try creating another account with the same email but a different password.

2. **Form Submission**:
   - Fill the registration form with a long string in the password field.
   - Submit the form and check for server errors or unexpected behavior.

3. **Email Verification**:
   - Enter an invalid or malformed email address.
   - Check if the system properly validates the email.

4. **Password Policy**:
   - Test if the system allows weak or easily guessable passwords.
   - Check if the password can be the same as the email address.

---
-- https://xmind.ai/share/6ZrhpTgT?xid=JCZJS2jB
- https://www.irongeek.com/homoglyph-attack-generator.php

  ------
### **Email and OTP Bypass Techniques**

Here’s a **summarized and organized version** of the email and OTP bypass techniques you provided:

---

#### **1. Throwaway Email Services**
Use temporary email services for testing:  
- [ProtonMail](https://mail.protonmail.com)  
- [GetAirMail](http://en.getairmail.com)  
- [Temp-Mail](https://temp-mail.org/en)  
- [Mailinator](https://www.mailinator.com)  

---

#### **2. Username/Email Enumeration**
- **Non-Brute Force Methods**:  
  - Check registration processes for IDOR or endpoints leaking usernames/emails.  

---

#### **3. SQL Injection in Email Field**
- **Payloads**:  
  ```json
  {"email":"asd'or'1'='1@a.com"}  // Valid
  {"email":"a'-IF(LENGTH(database())>9,SLEEP(7),0)or'1'='1@a.com"}  // Delay-based
  ```
- **Reference**: [Bypassing Email Filter SQL Injection](https://dimazarno.medium.com/bypassing-email-filter-which-leads-to-sql-injection-e57bcbfc6b17)  

---

#### **4. Email Verification Bypass**
1. **Privilege Escalation**:  
   - Change email to victim’s email during signup.  
   - Wait for verification link to arrive in your inbox.  
   - Example: Shopify trial account takeover.  

2. **Link Doesn’t Expire**:  
   - Save confirmation link without verifying.  
   - Use the link after the victim changes their email.  

3. **OAuth Bypass**:  
   - Sign up with the same email using OAuth and email signup.  
   - Example: [HackerOne Report](https://hackerone.com/reports/1074047).  

4. **Leaked Verification Link**:  
   - Check server responses for verification links.  

5. **Response Manipulation**:  
   - Intercept and modify the response to bypass verification.  
   - Example: Change `{"status":false}` to `{"status":true}`.  

6. **No Rate Limit**:  
   - Resend confirmation emails multiple times to flood the victim.  
   - Example: [HackerOne Report](https://hackerone.com/reports/774050).  

7. **Broken Authentication**:  
   - Change email back to the original after verification.  
   - Exploit improper email verification logic.  

---

#### **5. ATO via Email Parameter Manipulation**
- **Parameter Pollution**:  
  ```plaintext
  email=victim@mail.com&email=hacker@mail.com
  ```
- **Array of Emails**:  
  ```json
  {"email":["victim@mail.com","hacker@mail.com"]}
  ```
- **Carbon Copy (CC/BCC)**:  
  ```plaintext
  email=victim@mail.com%0A%0Dcc:hacker@mail.com
  ```
- **Separators**:  
  ```plaintext
  email=victim@mail.com,hacker@mail.com
  email=victim@mail.com|hacker@mail.com
  ```

---

#### **6. OTP Bypass Techniques**
1. **Response Manipulation**:  
   - Intercept and modify the OTP verification response.  
   - Example: Change `{"status":0}` to `{"status":1}`.  

2. **Repeating Form Submission**:  
   - Use Burp Repeater to submit OTP requests multiple times.  

3. **No Rate Limit**:  
   - Brute-force OTP by sending repeated requests.  

4. **Default OTPs**:  
   - Test common OTPs like `111111`, `123456`, `000000`.  

5. **Leaked OTP in Response**:  
   - Check server responses for OTP values.  

6. **Old OTP Still Valid**:  
   - Test if previously used OTPs are still accepted.  

---

#### **7. Duplicate Registration**
- **Overwrite Existing User**:  
  - Register with the same email but different password.  
  - Example: `abc@gmail.com` vs `Abc@gmail.com`.  

- **Exploit Case Sensitivity**:  
  - Register with uppercase/lowercase variations of the same email.  

- **Reference**: [Duplicate Registration Exploit](https://shahjerry33.medium.com/duplicate-registration-the-twinning-twins-883dfee59eaf)  

---

#### **8. DOS in Registration**
- **Long String in Password Field**:  
  - Submit a very long password string to crash the server.  
  - Example: [HackerOne Report](https://hackerone.com/reports/738569).  

---

#### **9. Path Overwrite**
- **Reserved Filenames**:  
  - Register with usernames like `index.php`, `login.php`.  
  - Overwrite system pages with user profiles.  
  - Reference: [Path Hijacking](https://infosecwriteups.com/logical-flaw-resulting-path-hijacking-dd4d1e1e832f)  

---

#### **10. Weak Password Policy**
- **Common Issues**:  
  - Accepting weak passwords like `123456`.  
  - Allowing passwords same as email or username.  
  - Improper password reset/change implementations.
 
---

## register vulnerability
[ ] Duplicate registration overwrite existing user
```
1. create first account in application with email say abc@gmail.com and password
2. logout of the account and create another account with same email and different password
3. you can even try to change email case like from abc2gmail.com to Abc@gmail.com
4. finish the creation proccess and see that it succceed
5. now go back and try to login with email and the new password ,you are seccess logged in
```
[ ] Dos at name /password field in sign up page
```
1. go to sign up form
2. fill the form and enter a long string in password 
3. click on enter and you will get 500 internal server error if it is vulnerability
```

[ ] no rate limit at signup page
```
1. enter your details in signuo form and submit the form
2. capture the signuo request and send it to intruder
3. add $$ to email parameter
4. in the payload add different email address
5. fire up intruder and check whether it return 200 ok
```

[ ] xss in username,email
```
xss can be test in any of parameter
1. payload for text field:
2. payload for email field:
3. you can use bypassing filter
```

[ ] email varification can be easily bypassed with following method
```
1. response manipulation change the bad respone with good one like false to true
2. status code manipulation change the 403 to 200
```

[ ] weak register implemntation
```
1. check whether the allows disposable email addresses
2. register form on non-https page
```

[ ] weak password policy
```
1. check whether application allows easily guessable passsword like 123456
2. check if you can use username same as the email address
3. check if can use password same as that email address
4. improperly implemented password recovery link functionality
```

[ ] Path Overwrite
```
If an application allows users to check their profile with direct path /{username} always try to signup with system reserved file names, such as index.php, signup.php, login.php, etc. In some cases what happens here is, when you signup with username: index.php, now upon visiting target.tld/index.php, your profile will comeup and occupy the index.php page of an application. Similarly, if an attacker is able to signup with username login.php, Imagine login page getting takeovered.
```

---
- https://portswigger.net/research/bypassing-character-blocklists-with-unicode-overflows
  
