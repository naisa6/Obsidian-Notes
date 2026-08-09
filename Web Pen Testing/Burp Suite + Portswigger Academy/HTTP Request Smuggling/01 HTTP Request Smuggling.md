## What is HTTP request smuggling?

HTTP request smuggling is a technique for interfering with the way a web site processes sequences of HTTP requests that are received from one or more users. Request smuggling vulnerabilities are often critical in nature, allowing an attacker to bypass security controls, gain unauthorized access to sensitive data, and directly compromise other application users.

Request smuggling is primarily associated with HTTP/1 requests. However, websites that support HTTP/2 may be vulnerable, depending on their back-end architecture.

## What happens in an HTTP request smuggling attack?

Today's web applications frequently employ chains of HTTP servers between users and the ultimate application logic. Users send requests to a front-end server (sometimes called a load balancer or reverse proxy) and this server forwards requests to one or more back-end servers. This type of architecture is increasingly common, and in some cases unavoidable, in modern cloud-based applications.

When the front-end server forwards HTTP requests to a back-end server, it typically sends several requests over the same back-end network connection, because this is much more efficient and performant. The protocol is very simple; HTTP requests are sent one after another, and the receiving server has to determine where one request ends and the next one begins:

![[forwarding-http-requests-to-back-end-server.svg]]
In this situation, it is crucial that the front-end and back-end systems agree about the boundaries between requests. Otherwise, an attacker might be able to send an ambiguous request that gets interpreted differently by the front-end and back-end systems:
![[smuggling-http-request-to-back-end-server.svg]]

Here, the attacker causes part of their front-end request to be interpreted by the back-end server as the start of the next request. It is effectively prepended to the next request, and so can interfere with the way the application processes that request. This is a request smuggling attack, and it can have devastating results.

## How do HTTP request smuggling vulnerabilities arise?

Most HTTP request smuggling vulnerabilities arise because the HTTP/1 specification provides two different ways to specify where a request ends: the `Content-Length` header and the `Transfer-Encoding` header.

The `Content-Length` header is straightforward: it specifies the length of the message body in bytes. For example:

`POST /search HTTP/1.1 Host: normal-website.com Content-Type: application/x-www-form-urlencoded Content-Length: 11 q=smuggling`

The `Transfer-Encoding` header can be used to specify that the message body uses chunked encoding. This means that the message body contains one or more chunks of data. Each chunk consists of the chunk size in bytes (expressed in hexadecimal), followed by a newline, followed by the chunk contents. The message is terminated with a chunk of size zero. For example:

`POST /search HTTP/1.1 Host: normal-website.com Content-Type: application/x-www-form-urlencoded Transfer-Encoding: chunked b q=smuggling 0`

#### Note

Many security testers are unaware that chunked encoding can be used in HTTP requests, for two reasons:

- Burp Suite automatically unpacks chunked encoding to make messages easier to view and edit.
- Browsers do not normally use chunked encoding in requests, and it is normally seen only in server responses.

As the HTTP/1 specification provides two different methods for specifying the length of HTTP messages, it is possible for a single message to use both methods at once, such that they conflict with each other. The specification attempts to prevent this problem by stating that if both the `Content-Length` and `Transfer-Encoding` headers are present, then the `Content-Length` header should be ignored. This might be sufficient to avoid ambiguity when only a single server is in play, but not when two or more servers are chained together. In this situation, problems can arise for two reasons:

- Some servers do not support the `Transfer-Encoding` header in requests.
- Some servers that do support the `Transfer-Encoding` header can be induced not to process it if the header is obfuscated in some way.

If the front-end and back-end servers behave differently in relation to the (possibly obfuscated) `Transfer-Encoding` header, then they might disagree about the boundaries between successive requests, leading to request smuggling vulnerabilities.

#### Note

Websites that use HTTP/2 end-to-end are inherently immune to request smuggling attacks. As the HTTP/2 specification introduces a single, robust mechanism for specifying the length of a request, there is no way for an attacker to introduce the required ambiguity.

However, many websites have an HTTP/2-speaking front-end server, but deploy this in front of back-end infrastructure that only supports HTTP/1. This means that the front-end effectively has to translate the requests it receives into HTTP/1. This process is known as HTTP downgrading. For more information, see [Advanced request smuggling](https://portswigger.net/web-security/request-smuggling/advanced).

## Using HTTP request smuggling to perform web cache deception

In yet another variant of the attack, you can leverage HTTP request smuggling to perform web cache deception. This works in a similar way to the web cache poisoning attack but with a different purpose.

#### What is the difference between web cache poisoning and web cache deception?

- In **web cache poisoning**, the attacker causes the application to store some malicious content in the cache, and this content is served from the cache to other application users.
- In **web cache deception**, the attacker causes the application to store some sensitive content belonging to another user in the cache, and the attacker then retrieves this content from the cache.

In this variant, the attacker smuggles a request that returns some sensitive user-specific content. For example:

`POST / HTTP/1.1 Host: vulnerable-website.com Content-Length: 43 Transfer-Encoding: chunked 0 GET /private/messages HTTP/1.1 Foo: X`

The next request from another user that is forwarded to the back-end server will be appended to the smuggled request, including session cookies and other headers. For example:

`GET /private/messages HTTP/1.1 Foo: XGET /static/some-image.png HTTP/1.1 Host: vulnerable-website.com Cookie: sessionId=q1jn30m6mqa7nbwsa0bhmbr7ln2vmh7z ...`

The back-end server responds to this request in the normal way. The URL in the request is for the user's private messages and the request is processed in the context of the victim user's session. The front-end server caches this response against what it believes is the URL in the second request, which is `/static/some-image.png`:

`GET /static/some-image.png HTTP/1.1 Host: vulnerable-website.com HTTP/1.1 200 Ok ... <h1>Your private messages</h1> ...`

The attacker then visits the static URL and receives the sensitive content that is returned from the cache.

An important caveat here is that the attacker doesn't know the URL against which the sensitive content will be cached, since this will be whatever URL the victim user happened to be requesting when the smuggled request took effect. The attacker might need to fetch a large number of static URLs to discover the captured content.

### H2.CL vulnerabilities

HTTP/2 requests don't have to specify their length explicitly in a header. During downgrading, this means front-end servers often add an HTTP/1 `Content-Length` header, deriving its value using [HTTP/2's built-in length mechanism](https://portswigger.net/web-security/request-smuggling/advanced#http-2-message-length). Interestingly, HTTP/2 requests can also include their own `content-length` header. In this case, some front-end servers will simply reuse this value in the resulting HTTP/1 request.

The spec dictates that any `content-length` header in an HTTP/2 request must match the length calculated using the built-in mechanism, but this isn't always validated properly before downgrading. As a result, it may be possible to smuggle requests by injecting a misleading `content-length` header. Although the front-end will use the implicit HTTP/2 length to determine where the request ends, the HTTP/1 back-end has to refer to the `Content-Length` header derived from your injected one, resulting in a desync.

**Front-end (HTTP/2)**

|   |   |
|---|---|
|:method|POST|
|:path|/example|
|:authority|vulnerable-website.com|
|content-type|application/x-www-form-urlencoded|
|content-length|0|
|GET /admin HTTP/1.1 Host: vulnerable-website.com Content-Length: 10 x=1|   |

**Back-end (HTTP/1)**

`POST /example HTTP/1.1 Host: vulnerable-website.com Content-Type: application/x-www-form-urlencoded Content-Length: 0 GET /admin HTTP/1.1 Host: vulnerable-website.com Content-Length: 10 x=1GET / H`

When performing some request smuggling attacks, you will want headers from the victim's request to be appended to your smuggled prefix. However, these can interfere with your attack in some cases, resulting in duplicate header errors and suchlike. In the example above, we've mitigated this by including a trailing parameter and a `Content-Length` header in the smuggled prefix. By using a `Content-Length` header that is slightly longer than the body, the victim's request will still be appended to your smuggled prefix but will be truncated before the headers.

### H2.TE vulnerabilities

Chunked transfer encoding is incompatible with HTTP/2 and the spec recommends that any `transfer-encoding: chunked` header you try to inject should be stripped or the request blocked entirely. If the front-end server fails to do this, and subsequently downgrades the request for an HTTP/1 back-end that does support chunked encoding, this can also enable request smuggling attacks.

**Front-end (HTTP/2)**

|   |   |
|---|---|
|:method|POST|
|:path|/example|
|:authority|vulnerable-website.com|
|content-type|application/x-www-form-urlencoded|
|transfer-encoding|chunked|
|0 GET /admin HTTP/1.1 Host: vulnerable-website.com Foo: bar|   |

**Back-end (HTTP/1)**

`POST /example HTTP/1.1 Host: vulnerable-website.com Content-Type: application/x-www-form-urlencoded Transfer-Encoding: chunked 0 GET /admin HTTP/1.1 Host: vulnerable-website.com Foo: bar`

If a website is vulnerable to either H2.CL or H2.TE request smuggling, you can potentially leverage this behavior to perform the same attacks that we covered in our [previous request smuggling labs](https://portswigger.net/web-security/request-smuggling).

### Hidden HTTP/2 support

Browsers and other clients, including Burp, typically only use HTTP/2 to communicate with servers that explicitly advertise support for it via ALPN as part of the TLS handshake.

Some servers support HTTP/2 but fail to declare this properly due to misconfiguration. In such cases, it can appear as though the server only supports HTTP/1.1 because clients default to this as a fallback option. As a result, testers may overlook viable HTTP/2 attack surface and miss protocol-level issues, such as the examples of HTTP/2 downgrade-based request smuggling that we covered above.

To force Burp Repeater to use HTTP/2 so that you can test for this misconfiguration manually:

1. From the **Settings** dialog, go to **Tools > Repeater**.
2. Under **Connections**, enable the **Allow HTTP/2 ALPN override** option.
3. In Repeater, go to the **Inspector** panel and expand the **Request attributes** section.
4. Use the switch to set the **Protocol** to **HTTP/2**. Burp will now send all requests on this tab using HTTP/2, regardless of whether the server advertises support for this.

#### Note

If you're using Burp Suite Professional, Burp Scanner automatically detects instances of hidden HTTP/2 support.

## Request smuggling via CRLF injection

Even if websites take steps to prevent basic H2.CL or H2.TE attacks, such as validating the `content-length` or stripping any `transfer-encoding` headers, HTTP/2's binary format enables some novel ways to bypass these kinds of front-end measures.

In HTTP/1, you can sometimes exploit discrepancies between how servers handle standalone newline (`\n`) characters to smuggle prohibited headers. If the back-end treats this as a delimiter, but the front-end server does not, some front-end servers will fail to detect the second header at all.

`Foo: bar\nTransfer-Encoding: chunked`

This discrepancy doesn't exist with the handling of a full CRLF (`\r\n`) sequence because all HTTP/1 servers agree that this terminates the header.

On the other hand, as HTTP/2 messages are binary rather than text-based, the boundaries of each header are based on explicit, predetermined offsets rather than delimiter characters. This means that `\r\n` no longer has any special significance within a header value and, therefore, can be included **inside** the value itself without causing the header to be split:

|   |   |
|---|---|
|foo|bar\r\nTransfer-Encoding: chunked|

This may seem relatively harmless on its own, but when this is rewritten as an HTTP/1 request, the `\r\n` will once again be interpreted as a header delimiter. As a result, an HTTP/1 back-end server would see two distinct headers:

`Foo: bar Transfer-Encoding: chunked`

## HTTP/2 request splitting

When we looked at [response queue poisoning](https://portswigger.net/web-security/request-smuggling/advanced/response-queue-poisoning), you learned how to split a single HTTP request into exactly two complete requests on the back-end. In the example we looked at, the split occurred inside the message body, but when HTTP/2 downgrading is in play, you can also cause this split to occur in the headers instead.

This approach is more versatile because you aren't dependent on using request methods that are allowed to contain a body. For example, you can even use a `GET` request:

|   |   |
|---|---|
|:method|GET|
|:path|/|
|:authority|vulnerable-website.com|
|foo|bar\r\n \r\n GET /admin HTTP/1.1\r\n Host: vulnerable-website.com|

This is also useful in cases where the `content-length` is validated and the back-end doesn't support chunked encoding.

### Accounting for front-end rewriting

To split a request in the headers, you need to understand how the request is rewritten by the front-end server and account for this when adding any HTTP/1 headers manually. Otherwise, one of the requests may be missing mandatory headers.

For example, you need to ensure that both requests received by the back-end contain a `Host` header. Front-end servers typically strip the `:authority` pseudo-header and replace it with a new HTTP/1 `Host` header during downgrading. There are different approaches for doing this, which can influence where you need to position the `Host` header that you're injecting.

Consider the following request:

|   |   |
|---|---|
|:method|GET|
|:path|/|
|:authority|vulnerable-website.com|
|foo|bar\r\n \r\n GET /admin HTTP/1.1\r\n Host: vulnerable-website.com|

During rewriting, some front-end servers append the new `Host` header to the end of the current list of headers. As far as an HTTP/2 front-end is concerned, this after the `foo` header. Note that this is also after the point at which the request will be split on the back-end. This means that the first request would have no `Host` header at all, while the smuggled request would have two. In this case, you need to position your injected `Host` header so that it ends up in the first request once the split occurs:

|   |   |
|---|---|
|:method|GET|
|:path|/|
|:authority|vulnerable-website.com|
|foo|bar\r\n Host: vulnerable-website.com\r\n \r\n GET /admin HTTP/1.1|

You will also need to adjust the positioning of any internal headers that you want to inject in a similar manner.

In the example above, we've split the request in a way that triggers [response queue poisoning](https://portswigger.net/web-security/request-smuggling/advanced/response-queue-poisoning), but you can also smuggle prefixes for classic request smuggling attacks in this way. In this case, your injected headers may clash with the headers in the request that is appended to your prefix on the back-end, resulting in duplicate header errors or causing the request to be terminated in the wrong place. To mitigate this, you can include a trailing body parameter in the smuggled prefix along with a `Content-Length` header that is slightly longer than the body. The victim's request will still be appended to your smuggled prefix but will be truncated before the headers.