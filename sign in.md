- LDAP injection
---
Always try the "\" character in login entries. It can trigger an SQL.
curl -d 'username=1\&password=1\' -X POST https :// login(.)domain(.)com
---
- https://github.com/Az0x7/vulnerability-Checklist/blob/main/Api%20Authentication%20/Authentication.md
