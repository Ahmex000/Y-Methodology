

### **Bug Bounty Tips & Techniques**

#### **1. Write-Up**
- **LinkedIn Post**:
  ```bash
  https://www.linkedin.com/posts/abdallah-samir-0357a5214_bugbounty-bugbountytips-bughunting-activity-7176165725588365313-TQxe?utm_source=social_share&utm_medium=member_android&utm_campaign=copy_link
  ```

#### **2. JSON Endpoint Manipulation**
- **Add `.json` After Identifiers**:
  ```bash
  Add `.json` after user [id, username, any identifier] to test for JSON responses or potential vulnerabilities.
  ```

#### **3. Request Method Manipulation**
- **Change Request Method**:
  ```bash
  Change the request method (GET, PUT, DELETE, PATCH, etc.) to test for different behaviors or vulnerabilities.
  ```
- **Parameter Manipulation**:
  ```bash
  ?user_id=<attacker_id>&user_id=<victim_id>
  ```
- **Special Characters**:
  ```bash
  Use special characters like %0a, %20, %1c, %09, %00 to test for input validation issues.
  ```

---

### **How to Use This Information**
1. **JSON Endpoint Testing**:
   - Append `.json` to user identifiers to check for JSON responses or potential data exposure.

2. **Request Method Testing**:
   - Experiment with different HTTP methods (GET, PUT, DELETE, PATCH) to identify vulnerabilities like IDOR (Insecure Direct Object Reference).

3. **Parameter Manipulation**:
   - Manipulate parameters like `user_id` to test for access control issues.

4. **Special Characters**:
   - Use special characters to test for input validation and sanitization issues.
   - https://github.com/Az0x7/vulnerability-Checklist/blob/main/Api%20Authorization/Authorization.md

