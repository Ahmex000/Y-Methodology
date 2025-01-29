- nmap
  ```bash
  nmap Target -p- -sV -A -sC
  ```
  
  ```bash
  nmap -sV --script=http-enum <target-url>
  ```
  ```bash
  nmap -sV -sC -sS 87.251.41.115
  ```

---

- Nuclei
  ```bash
  nuclei -l -t ~/nuclei-templates/http/exposures/
   ```
---
