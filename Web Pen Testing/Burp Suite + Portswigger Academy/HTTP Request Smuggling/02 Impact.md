## How to prevent HTTP request smuggling vulnerabilities

HTTP request smuggling vulnerabilities arise in situations where the front-end server and back-end server use different mechanisms for determining the boundaries between requests. This may be due to discrepancies between whether HTTP/1 servers use the `Content-Length` header or chunked transfer encoding to determine where each request ends. In HTTP/2 environments, the common practice of [downgrading HTTP/2 requests](https://portswigger.net/web-security/request-smuggling/advanced/http2-downgrading) for the back-end is also fraught with issues and enables or simplifies a number of additional attacks.

To prevent HTTP request smuggling vulnerabilities, we recommend the following high-level measures:

- Use HTTP/2 end to end and disable HTTP downgrading if possible. HTTP/2 uses a robust mechanism for determining the length of requests and, when used end to end, is inherently protected against request smuggling. If you can't avoid HTTP downgrading, make sure you validate the rewritten request against the HTTP/1.1 specification. For example, reject requests that contain newlines in the headers, colons in header names, and spaces in the request method.
    
- Make the front-end server normalize ambiguous requests and make the back-end server reject any that are still ambiguous, closing the TCP connection in the process.
    
- Never assume that requests won't have a body. This is the fundamental cause of both CL.0 and client-side desync vulnerabilities.
    
- Default to discarding the connection if server-level exceptions are triggered when handling requests.
    
- If you route traffic through a forward proxy, ensure that upstream HTTP/2 is enabled if possible.
    

As we've demonstrated in the learning materials, disabling reuse of back-end connections will help to mitigate certain kinds of attack, but this still doesn't protect you from [request tunnelling](https://portswigger.net/web-security/request-smuggling/advanced/request-tunnelling) attacks.