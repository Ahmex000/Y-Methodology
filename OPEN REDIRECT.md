- https://github.com/cujanovic/Open-Redirect-Payloads/blob/master/Open-Redirect-payloads.txt
- try SSRF
- tray XSS

```jsx
trarget.\com/%09/myburpcollab
trarget.\com//myburpcollab
trarget.\com\\myburpcollab
target.\com/%0d/myburpcollab
redirect_uri=target.\com&redirect_uri=attacker\.com
redirect_uri=target.\com%40myburpcollab.\com
```

-
- https://portswigger.net/research/bypassing-character-blocklists-with-unicode-overflows
