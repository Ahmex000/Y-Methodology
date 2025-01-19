```jsx
Quick Oneliner to Check for CORS:

cat origins.txt | xargs -P10 -I{} bash -c 'curl -s -I -H "Origin: {}" -H "Access-Control-Request-Method: GET" https://target.com | grep -q "Access-Control-Allow-Origin: {}" && echo "[+] Vulnerable: {}"'
```

- 
-
