## **0365 Enumeration for Apex Domains**  

### **Why?**  
- Identify if a domain uses **Microsoft 365 (O365)** for email services.  
- Extract **DNS records (MX, SPF, DMARC, Autodiscover)** to analyze security settings.  
- Enumerate **valid O365 users** for phishing or password spraying attacks.  
- Find weak email configurations that allow **spoofing or unauthorized access**.  

### **1️⃣ Check MX Records for O365 Usage**  
If an organization uses **Microsoft 365**, its MX record will point to `mail.protection.outlook.com`.  

```sh
dig MX example.com +short
```

```sh
10 example-com.mail.protection.outlook.com.
```

### **2️⃣ Check Autodiscover for O365 Presence**  
The **Autodiscover service** is used to configure email clients. If enabled, it confirms O365 usage.  

```sh
curl -s -I https://autodiscover.example.com/autodiscover/autodiscover.xml
```

```sh
HTTP/1.1 401 Unauthorized
```

### **3️⃣ Validate O365 Domain with `GetO365Domain`**  
Use `GetO365Domain` to confirm if a domain is registered with O365.  

```sh
python3 GetO365Domain.py -d example.com
```

```sh
[+] The domain example.com is associated with Microsoft 365.
```

### **4️⃣ Enumerate O365 Users with `Office365UserEnum`**  
Use `Office365UserEnum` to discover **valid users** on an O365 domain.  

```sh
python3 Office365UserEnum.py -d example.com -u userlist.txt
```

```sh
[+] Found valid user: john.doe@example.com
[+] Found valid user: admin@example.com
```

### **5️⃣ Check SPF & DMARC for Email Security Weaknesses**  
Misconfigured SPF & DMARC can allow **email spoofing** and phishing attacks.  

```sh
dig TXT example.com +short | grep "spf"
```

```sh
dig TXT _dmarc.example.com +short
```

```sh
"v=spf1 include:spf.protection.outlook.com -all"
"v=DMARC1; p=none;"
```

### **Tools Used:**  
```sh
- dig → Extract MX, SPF, and DMARC records.  
- curl → Check Autodiscover availability.  
- GetO365Domain.py → Verify if a domain is registered with O365.  
- Office365UserEnum.py → Enumerate valid O365 users.  
```

### **Key Takeaways:**  
```sh
- Confirms if a domain is using Microsoft 365.  
- Extracts valid O365 users for further attacks (phishing, password spraying).  
- Identifies email security misconfigurations (SPF, DMARC) that allow spoofing.  
- Expands attack surface by analyzing email infrastructure.  
```

🚀 **0365 Enumeration is a critical step in email security assessments and recon.**  
