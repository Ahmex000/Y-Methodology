- LDAP injection
---
Always try the "\" character in login entries. It can trigger an SQL.
curl -d 'username=1\&password=1\' -X POST https :// login(.)domain(.)com
---
