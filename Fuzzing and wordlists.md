
---

### **Fuzzing Techniques**

#### **1. FFuf Commands**
- Basic Fuzzing:
  ```bash
  ffuf -u http://target.com/FUZZ -w yourlist.txt
  ```
- Fuzzing with Host Header:
  ```bash
  ffuf -u http://target.com/FUZZ -H "Host: 127.0.0.1" -w yourlist.txt
  ffuf -u http://target.com/FUZZ -H "Host: localhost" -w yourlist.txt
  ```
- Advanced Fuzzing:
  ```bash
  ffuf -u http://target.com/;/FUZZ/ -w yourlist.txt
  ffuf -u http://target.com/%2e/FUZZ/ -w yourlist.txt
  ffuf -u http://target.com/..238/FUZZ/ -w yourlist.txt
  ffuf -u http://target.com/%2e%2eX2f/FUZZ/ -w yourlist.txt
  ffuf -u http://target.com/_FUZZ -w yourlist.txt
  ffuf -u http://target.com/%u082e%u002¢/%u002e%u002€/FUZZ -w yourlist.txt
  ffuf -u http://target.com/FUZZ_ -w yourlist.txt
  ffuf -u http://target.com/_FUZZ_ -w yourlist.txt
  ffuf -u http://target.com/..;/FUZZ/ -w yourlist.txt
  ffuf -u http://target.com/..;/..;/FUZZ/ -w yourlist.txt
  ffuf -u http://target.com/../FUZZ -w yourlist.txt
  ffuf -u http://target.com/FUZZ..;/ -w yourlist.txt
  ffuf -u http://target.com/FUZZ;/ -w yourlist.txt
  ffuf -u http://target.com/FUZZ# -w yourlist.txt
  ffuf -u http://target.com/FUZZ/~ -w yourlist.txt
  ffuf -u http://target.com/!FUZZ -w yourlist.txt
  ffuf -u http://target.com/#/FUZZ/ -w yourlist.txt
  ffuf -u http://target.com/-/FUZZ/ -w yourlist.txt
  ffuf -u http://target.com/FUZZ.bkp -w yourlist.txt
  ffuf -u http://target.com/FUZZA20 -w yourlist.txt
  ffuf -u http://target.com/FUZZ.something -w yourlist.txt
  ffuf -u http://target.com/FUZZ~ -w yourlist.txt
  ffuf -u http://target.com/FUZZ/.git/config -w yourlist.txt
  ffuf -u http://target.com/FUZZ/.env -w yourlist.txt
  ffuf -u http://target.com/FUZZ. -w yourlist.txt
  ffuf -u http://target.com/FUZZ/* -w yourlist.txt
  ffuf -u http://target.com/FUZZ/? -w yourlist.txt
  ffuf -u http://target.com/FUZZ -recursive -w fuzzing.txt
  ffuf -u http://target.com/FUZZ -recursive -w fuzzing.txt -e txt,tree,main
  ffuf -u http://target.com/FUZZ?.css -w fuzzing.txt
  ```

#### **2. Wordlists and Parameters**
- **Seclists - Low Hang Fruit**:
  ```bash
  https://wordlists-cdn.assetnote.io/data/automated/httparchive_parameters_top_1m_2023_12_28.txt
  ```
- **Hidden Parameters**:
  ```bash
  https://github.com/Bo0oM/ParamPamPam/blob/master/params.txt
  ```
- **OneListForAll**:
  ```bash
  https://github.com/six2dez/OneListForAll
  ```
- **API**:
  ```bash
  https://wordlists-cdn.assetnote.io/data/automated/
  ```

#### **3. Mind Maps and Learning Resources**
- **Mind Maps**:
  ```bash
  https://github.com/imran-parray/Mind-Maps
  ```
- **How to Fuzz Smart**:
  ```bash
  https://medium.com/@gilsgil/smart-fuzzing-finding-bugs-like-no-one-else-by-gilson-oliveira-d6aa0dbc285b
  ```

#### **4. Tools**
- **Lazy Recon**
- **Link Finder**
- **SubDomain Takeover**
- **sud404**
- **Commonspeak**:
  ```bash
  https://github.com/pentester-io/commonspeak
  ```
- **Shortscan**
- **CWFF**:
  ```bash
  https://github.com/D4Vinci/CWFF
  ```
- **Feroxbuster**:
  ```bash
  https://github.com/epi052/feroxbuster
  ```

#### **5. Fuzzing Techniques**
- **Dirsearch**:
  ```bash
  dirsearch -u domain --deep-recursive
  ```
- **Process Substitution**:
  ```bash
  ffuf -c -w <(seq 1 1337) -u https://company.com/api/users/FUZZ
  ```
- **Escape Characters in Endpoints**:
  ```bash
  ffuf -u http://target.com/users/\passwords/ -w wordlist.txt
  ```
- **Fuzzing Local Subdomains**:
  ```bash
  ffuf -H "HOST: localsubdomain.com" -w wordlist.txt -u http://ip/FUZZ
  ```

#### **6. Additional Resources**
- **Radamsa**:
  ```bash
  https://gitlab.com/akihe/radamsa
  ```

#### **7. Nuclei Fuzzing Templates**
- **NucleiFuzzer**:
  ```bash
  https://github.com/0xKayala/NucleiFuzzer
  ```
- **Fuzz Templates**:
  ```bash
  https://github.com/U53RW4R3/nuclei-fuzzer-templates
  https://github.com/projectdiscovery/fuzzing-templates
  https://github.com/pikpikcu/nuclei-fuzz
  https://github.com/vijay922/XXE-FUZZ
  https://github.com/MR-pentestGuy/nuclei-templates
  ```
-- https://gowthams.gitbook.io/bughunter-handbook/fuzzing-wordlists
-- https://bugbountyforum.com/tools/recon/

