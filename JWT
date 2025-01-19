---

### **JWT (JSON Web Tokens) Testing Scenarios**

1. **General Testing**:
   - Open Medium and try different scenarios.
   - Look for differences between JWTs.
   - Try changing the body of the JWT.
   - Try deleting the signature key.
   - Try generating a new key.
   - Search on Google for new methods and take notes.

2. **Tools**:
   - Use **John the Ripper** for cracking JWT secrets.
   - Use **jwt_tool** for testing JWT vulnerabilities:
     - [jwt_tool GitHub](https://github.com/ticarpi/jwt_tool)
     - [jwt_tool Attack Methodology](https://github.com/ticarpi/jwt_tool/wiki/Attack-Methodology)

3. **Specific Issues**:
   - If you delete `Bearer` from the **Authorization** header, you might authenticate as an admin on `https://admin.test.com` and gain admin permissions.

4. **References**:
   - RFC 7519 (JWT Specification):
     - [RFC 7519 - JWT](https://repository.root-me.org/RFC/EN%20-%20rfc7519.txt)
   - Critical vulnerabilities in JWT libraries:
     - [Auth0 Blog - JWT Vulnerabilities](https://auth0.com/blog/critical-vulnerabilities-in-json-web-token-libraries/)

5. **Advanced Testing**:
   - Test for **SQL Injection (SQLi)** in JWT headers.
   - Test for **Local File Inclusion (LFI)** in JWT headers.

---
