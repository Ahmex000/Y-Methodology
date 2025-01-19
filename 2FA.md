### **2FA Bypass Testing Scenarios**

#### **General Research**
- Check **Medium** for articles and writeups.
- Check **GitHub** for tools and repositories.
- Check **Telegram** for tips and discussions.
- Check **X (Twitter)** for tips and updates.
- Search on **Google** for new techniques and vulnerabilities.

#### **Writeups & References**
- **Google Auth Bypass Writeup**: [Article Link](https://sankalpabaral.medium.com/bypassing-2fa-my-unforgettable-experience-6e4b98615485)
- **2FA Bypass Techniques Writeup**: [Article Link](https://apexvicky.medium.com/2fa-bypass-techniques-dcdb19d29f11)
- **2FA Bypass Blog**: [Article Link](https://securitycipher.com/docs/2fa-bypass/)
- **2FA Bypass Article**: [Article Link](https://rashahacks.com/bypass-2fa-mechanism/)

#### **Additional Resources**
- **2FA Bypass Mind Map**: [Mind Map Link](https://xmind.ai/share/UYJM9L9t)
- **2FA Bypass Mind Map 2**: [Mind Map Link](https://xmind.ai/share/ph6inJpw?xid=HPdwd04O)

---

### **Summary from the Additional Link (https://sallam.gitbook.io/sec-88/web-appsec/features-abuse/2fa)**

#### **1. 2FA Bypass Testing Methods**
- **Reusing the Code**: Try reusing a previously sent 2FA code.
- **State Change**: Change the user’s state (e.g., disable 2FA) after login.
- **Bypassing Verification**: Attempt to bypass the verification page by manipulating requests.
- **Time Manipulation**: Try extending the validity period of the 2FA code or resending it.
- **Protocol Exploitation**: Exploit vulnerabilities in protocols like OAuth or SAML.

#### **2. Common Techniques**
- **Response Manipulation**: Modify server responses to bypass 2FA.
- **Parameter Tampering**: Alter parameters in requests to skip 2FA.
- **Brute Force**: Attempt to brute-force 2FA codes (if rate limits are weak).
- **Session Hijacking**: Steal active sessions to bypass 2FA.

#### **3. Tools for Testing**
- **Burp Suite**: For intercepting and manipulating requests.
- **OAuth Toolkit**: For testing OAuth implementations.
- **Custom Scripts**: To automate testing of 2FA bypass techniques.

---

----------------------

---

### **2FA Bypass Testing Scenarios**

---

#### **1. 2FA Setup Vulnerabilities**

1. **2FA Secret Cannot Be Rotated [P4]**  
   - Steps:  
     1. Log in and set up 2FA.  
     2. Observe if the 2FA secret cannot be rotated or regenerated.  
   - References:  
     - [Bugcrowd Disclosure](https://bugcrowd.com/disclosures/0c8a87aa-f10f-4174-b6d8-56c365062910/2fa-secret-is-not-rotated)  
     - [Zofixer Article](https://zofixer.com/what-is-weak-2fa-implementation-2fa-secret-cannot-be-rotated-vulnerability/)  

2. **2FA Secret Remains Obtainable After 2FA is Enabled [P4]**  
   - Steps:  
     1. Check if the QR code or secret is leaked during 2FA setup.  
     2. Analyze JavaScript files to understand secret generation.  
     3. Test if the secret is still retrievable after activation (e.g., via replay attacks).  
   - References:  
     - [Bugcrowd Taxonomy](https://bugcrowd.com/vulnerability-rating-taxonomy)  
     - [GitHub Issue](https://github.com/bugcrowd/vulnerability-rating-taxonomy/issues/203)  

3. **2FA Setup Logic Flaw [Variant]**  
   - Steps:  
     1. Initiate 2FA setup and provide an incorrect code.  
     2. Intercept and manipulate the response to indicate success.  
   - Impact: Users may be locked out of their accounts.  

4. **Old Session Does Not Expire After 2FA Setup [P4]**  
   - Steps:  
     1. Log in on two browsers and enable 2FA in one session.  
     2. Check if the second session remains active.  
   - Impact: Attackers can hijack pre-2FA sessions.  
   - Reference: [Bugcrowd Disclosure](https://bugcrowd.com/disclosures/4147cfbb-a808-4504-9b4f-2a8b68e17d62/old-session-does-not-expire-after-setup-2fa)  

5. **Enable 2FA Without Verifying Email [P3]**  
   - Steps:  
     1. Sign up with a victim’s email (without verification).  
     2. Enable 2FA on the unverified account.  
   - Impact: Victim cannot register or log in.  
   - Reference: [HackerOne Report](https://hackerone.com/reports/649533)  

6. **IDOR Leads to ATO [P2, P3]**  
   - Steps:  
     1. Register two accounts (user1 and user2).  
     2. Perform 2FA setup for user1 using user2’s ID.  
   - Impact: 2FA is enabled on user1’s account without consent.  
   - Reference: [HackerOne Report](https://hackerone.com/reports/810880)  

---

#### **2. 2FA Bypass Techniques**

1. **2FA Code Not Updated After New Code is Requested [P5]**  
   - Steps:  
     1. Request a new 2FA code during login.  
     2. Check if the old code remains valid.  
   - Impact: Easier brute-forcing of 2FA codes.  
   - Reference: [GitHub Issue](https://github.com/bugcrowd/vulnerability-rating-taxonomy/issues/289)  

2. **Old 2FA Code Not Invalidated After New Code is Generated [P5, P4]**  
   - Steps:  
     1. Use an old 2FA code after a new one is generated.  
     2. Test if the old code still works after a long duration.  
   - References:  
     - [GitHub Issue](https://github.com/bugcrowd/vulnerability-rating-taxonomy/issues/289)  
     - [HackerOne Report](https://hackerone.com/reports/695041)  

3. **2FA Code Leakage in Response [P3]**  
   - Steps:  
     1. Trigger a 2FA code request (e.g., Send OTP).  
     2. Check if the code is leaked in the response.  
   - Reference: [HackerOne Report](https://hackerone.com/reports/1276373)  

4. **Lack of Brute-Force Protection [P4]**  
   - Steps:  
     1. Test for rate limits on 2FA code requests.  
     2. Attempt to brute-force valid 2FA codes.  
   - References:  
     - [HackerOne Report](https://hackerone.com/reports/1060518)  
     - [HackerOne Report](https://hackerone.com/reports/121696)  

5. **Missing 2FA Code Integrity Validation [P3]**  
   - Steps:  
     1. Use a valid 2FA code from one account on another account.  
     2. Check if the code bypasses 2FA protection.  

6. **Bypass 2FA with Null or 000000 [P3]**  
   - Steps:  
     1. Enter null, blank, or "000000" as the 2FA code.  
     2. Check if it bypasses 2FA.  

7. **2FA Referrer Check Bypass | Direct Request [P2, P3]**  
   - Steps:  
     1. Navigate directly to post-2FA pages.  
     2. Manipulate the Referrer header to bypass checks.  

8. **Misconfiguration of Session Permissions [P4, P3]**  
   - Steps:  
     1. Complete 2FA on one account.  
     2. Use the same session to access another account’s post-2FA flow.  

9. **Changing 2FA Mode Leads to Bypass [P3]**  
   - Steps:  
     1. Intercept the 2FA request and change the mode (e.g., SMS to email).  
   - Reference: [HackerOne Report](https://hackerone.com/reports/665722)  

10. **Bypass Using OAuth [P5]**  
    - Steps:  
      1. Use OAuth to log in without 2FA.  
    - Reference: [HackerOne Report](https://hackerone.com/reports/178293)  

11. **Random Timeout Issue on 2FA Endpoint [P3]**  
    - Steps:  
      1. Enter wrong 2FA codes in quick succession.  
      2. Check if the 2FA process is bypassed.  
    - Reference: [HackerOne Report](https://hackerone.com/reports/1747978)  

12. **Remove 2FA Cookie Part [P3]**  
    - Steps:  
      1. Remove or manipulate the cookie responsible for 2FA.  
    - Reference: [HackerOne Report](https://hackerone.com/reports/2315420)  

---

#### **3. Disabling 2FA Vulnerabilities**

1. **Lack of Brute-Force Protection on Disable 2FA [P4]**  
   - Steps:  
     1. Attempt to brute-force the disable 2FA endpoint.  
   - Reference: [HackerOne Report](https://hackerone.com/reports/1465277)  

2. **Disable 2FA via CSRF [P4]**  
   - Steps:  
     1. Generate a CSRF PoC for the disable 2FA endpoint.  
     2. Execute the PoC on the victim’s session.  
   - References:  
     - [Medium Article](https://vbharad.medium.com/2-fa-bypass-via-csrf-attack-8f2f6a6e3871)  
     - [HackerOne Report](https://hackerone.com/reports/670329)  

3. **Password Reset/Email Check → Disable 2FA [P5, P4]**  
   - Steps:  
     1. Reset the password and check if 2FA is disabled.  
   - Reference: [Infosec Writeup](https://infosecwriteups.com/how-i-bypass-2fa-while-resetting-password-3f73bf665728)  

4. **Logic Bug in Disabling 2FA [P3]**  
   - Steps:  
     1. Intercept the disable 2FA request and manipulate it.  
   - Reference: [HackerOne Report](https://hackerone.com/reports/783258)  

5. **Backup Code Abuse [Variant]**  
   - Steps:  
     1. Exploit backup codes using brute-force or response manipulation.  
   - References:  
     - [HackerOne Report](https://hackerone.com/reports/113953)  
     - [HackerOne Report](https://hackerone.com/reports/100509)  

6. **Password Not Checked When Disabling 2FA [P5, P4]**  
   - Steps:  
     1. Disable 2FA without providing a valid password.  
   - Reference: [HackerOne Report](https://hackerone.com/reports/587910)  

7. **Clickjacking on 2FA Disabling Page [P4]**  
   - Steps:  
     1. Test if the disable 2FA page is vulnerable to clickjacking.  

---
