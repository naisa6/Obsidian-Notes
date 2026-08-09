
## What is the DOM?

The Document Object Model (DOM) is a web browser's hierarchical representation of the elements on the page. Websites can use JavaScript to manipulate the nodes and objects of the DOM, as well as their properties. DOM manipulation in itself is not a problem. In fact, it is an integral part of how modern websites work. However, JavaScript that handles data insecurely can enable various attacks. DOM-based vulnerabilities arise when a website contains JavaScript that takes an attacker-controllable value, known as a source, and passes it into a dangerous function, known as a sink.

## Taint-flow vulnerabilities

Many DOM-based vulnerabilities can be traced back to problems with the way client-side code manipulates attacker-controllable data.

### What is taint flow?

To either exploit or mitigate these vulnerabilities, it is important to first familiarize yourself with the basics of taint flow between sources and sinks.

#### Sources

A source is a JavaScript property that accepts data that is potentially attacker-controlled. An example of a source is the `location.search` property because it reads input from the query string, which is relatively simple for an attacker to control. Ultimately, any property that can be controlled by the attacker is a potential source. This includes the referring URL (exposed by the `document.referrer` string), the user's cookies (exposed by the `document.cookie` string), and web messages.

#### Sinks

A sink is a potentially dangerous JavaScript function or DOM object that can cause undesirable effects if attacker-controlled data is passed to it. For example, the `eval()` function is a sink because it processes the argument that is passed to it as JavaScript. An example of an HTML sink is `document.body.innerHTML` because it potentially allows an attacker to inject malicious HTML and execute arbitrary JavaScript.

Fundamentally, DOM-based vulnerabilities arise when a website passes data from a source to a sink, which then handles the data in an unsafe way in the context of the client's session.

The most common source is the URL, which is typically accessed with the `location` object. An attacker can construct a link to send a victim to a vulnerable page with a payload in the query string and fragment portions of the URL. Consider the following code:

`goto = location.hash.slice(1) if (goto.startsWith('https:')) {   location = goto; }`

This is vulnerable to DOM-based open redirection because the `location.hash` source is handled in an unsafe way. If the URL contains a hash fragment that starts with `https:`, this code extracts the value of the `location.hash` property and sets it as the `location` property of the `window`. An attacker could exploit this vulnerability by constructing the following URL:

`https://www.innocent-website.com/example#https://www.evil-user.net`

When a victim visits this URL, the JavaScript sets the value of the `location` property to `https://www.evil-user.net`, which automatically redirects the victim to the malicious site. This behavior could easily be exploited to construct a phishing attack, for example.

### Common sources

The following are typical sources that can be used to exploit a variety of taint-flow vulnerabilities:

`document.URL document.documentURI document.URLUnencoded document.baseURI location document.cookie document.referrer window.name history.pushState history.replaceState localStorage sessionStorage IndexedDB (mozIndexedDB, webkitIndexedDB, msIndexedDB) Database`

The following kinds of data can also be used as sources to exploit taint-flow vulnerabilities:

- [Reflected data](https://portswigger.net/web-security/cross-site-scripting/dom-based#dom-xss-combined-with-reflected-and-stored-data) 
- [Stored data](https://portswigger.net/web-security/cross-site-scripting/dom-based#dom-xss-combined-with-reflected-and-stored-data) 
- [Web messages](https://portswigger.net/web-security/dom-based/controlling-the-web-message-source) 

# DOM-based open redirection

In this section, we'll talk about what DOM-based open redirection is, demonstrate how an attack is constructed, and suggest ways to reduce your exposure to DOM-based open-redirection vulnerabilities.

## What is DOM-based open redirection?

DOM-based open-redirection vulnerabilities arise when a script writes attacker-controllable data into a sink that can trigger cross-domain navigation. For example, the following code is vulnerable due to the unsafe way it handles the `location.hash` property:

`let url = /https?:\/\/.+/.exec(location.hash); if (url) {   location = url[0]; }`

An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will cause a redirection to an arbitrary external domain.







## What is DOM-based cookie manipulation?

Some DOM-based vulnerabilities allow attackers to manipulate data that they do not typically control. This transforms normally-safe data types, such as cookies, into potential sources. DOM-based cookie-manipulation vulnerabilities arise when a script writes attacker-controllable data into the value of a cookie.

An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will set an arbitrary value in the user's cookie. Many sinks are largely harmless on their own, but DOM-based cookie-manipulation attacks demonstrate how low-severity vulnerabilities can sometimes be used as part of an exploit chain for a high-severity attack. For example, if JavaScript writes data from a source into `document.cookie` without sanitizing it first, an attacker can manipulate the value of a single cookie to inject arbitrary values:

`document.cookie = 'cookieName='+location.hash.slice(1);`

If the website unsafely reflects values from cookies without HTML-encoding them, an attacker can use cookie-manipulation techniques to exploit this behavior.
## What is DOM-based document-domain manipulation?

Document-domain manipulation vulnerabilities arise when a script uses attacker-controllable data to set the `document.domain` property. An attacker may be able to use the vulnerability to construct a URL that, if visited by another user, will cause the response page to set an arbitrary `document.domain` value.

The `document.domain` property is used by browsers in their enforcement of the same origin policy. If two pages from different origins explicitly set the same `document.domain` value, then those two pages can interact in unrestricted ways. If an attacker can cause a page of a targeted website and another page they control (either directly, or via an XSS-like vulnerability) to set the same `document.domain` value, then the attacker may be able to fully compromise the target page via the page they already control. This opens up the same possibilities for exploitation as regular cross-site scripting (XSS) vulnerabilities.

Browsers generally enforce some restrictions on the values that can be assigned to `document.domain`, and may prevent the use of completely different values than the actual origin of the page. However, there are two important caveats to this. Firstly, browsers allow the use of child or parent domains, so an attacker may be able to switch the domain of the target page to that of a related website with a weaker security posture. Secondly, some browser quirks enable switching to completely unrelated domains. These caveats mean that the ability to manipulate the `document.domain` property of a page generally represents a security vulnerability whose severity is not far behind regular XSS.

## What is DOM-based WebSocket-URL poisoning?

WebSocket-URL poisoning occurs when a script uses controllable data as the target URL of a WebSocket connection. An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will cause the user's browser to open a WebSocket connection to a URL that is under the attacker's control.

## What is DOM-based link manipulation?

DOM-based link-manipulation vulnerabilities arise when a script writes attacker-controllable data to a navigation target within the current page, such as a clickable link or the submission URL of a form. An attacker might be able to use this vulnerability to construct a URL that, if visited by another application user, will modify the target of links within the response.

## What is DOM-based web message manipulation?

Web message vulnerabilities arise when a script sends attacker-controllable data as a web message to another document within the browser. An attacker may be able to use the web message data as a source by constructing a web page that, if visited by a user, will cause the user's browser to send a web message containing data that is under the attacker's control. For more information about the using web messages as a source, please refer to the [Controlling the web-message source](https://portswigger.net/web-security/dom-based/controlling-the-web-message-source) page.

## What is DOM-based Ajax request-header manipulation?

Using Ajax enables a website to make asynchronous requests to the server so that web applications can dynamically change content on the page without the need to reload the entire page. However, Ajax request-header manipulation vulnerabilities arise when a script writes attacker-controllable data into the request header of an Ajax request that is issued using an `XmlHttpRequest` object. An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will set an arbitrary header in the subsequent Ajax request. This can then be used as a starting point to chain together other kinds of attack, thereby increasing the potential severity of this vulnerability.

## What is DOM-based local file-path manipulation?

Local file-path manipulation vulnerabilities arise when a script passes attacker-controllable data to a file-handling API as the `filename` parameter. An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will cause the user's browser to open an arbitrary local file.


## What is DOM-based client-side SQL injection?

Client-side SQL-injection vulnerabilities arise when a script incorporates attacker-controllable data into a client-side SQL query in an unsafe way. An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will execute an arbitrary SQL query within the local SQL database of the user's browser.


## What is DOM-based HTML5-storage manipulation?

HTML5-storage manipulation vulnerabilities arise when a script stores attacker-controllable data in the HTML5 storage of the web browser (either `localStorage` or `sessionStorage`). An attacker may be able to use this behavior to construct a URL that, if visited by another user, will cause the user's browser to store attacker-controllable data.

This behavior does not in itself constitute a security vulnerability. However, if the application later reads data back from storage and processes it in an unsafe way, an attacker may be able to leverage the storage mechanism to deliver other DOM-based attacks, such as [cross-site scripting](https://portswigger.net/web-security/cross-site-scripting) and JavaScript injection.

## What is DOM-based XPath injection?

DOM-based XPath-injection vulnerabilities arise when a script incorporates attacker-controllable data into an XPath query. An attacker may be able to use this behavior to construct a URL that, if visited by another application user, will trigger the execution of an arbitrary XPath query, which could cause different data to be retrieved and processed by the website.

## What is DOM-based JSON injection?

DOM-based JSON-injection vulnerabilities arise when a script incorporates attacker-controllable data into a string that is parsed as a JSON data structure and then processed by the application. An attacker may be able to use this behavior to construct a URL that, if visited by another user, will cause arbitrary JSON data to be processed.

## What is DOM-data manipulation?

DOM-data manipulation vulnerabilities arise when a script writes attacker-controllable data to a field within the DOM that is used within the visible UI or client-side logic. An attacker may be able to use this vulnerability to construct a URL that, if visited by another user, will modify the appearance or behavior of the client-side UI. DOM-data manipulation vulnerabilities can be exploited by both reflected and stored DOM-based attacks.

## What is DOM-based denial of service?

DOM-based denial-of-service vulnerabilities arise when a script passes attacker-controllable data in an unsafe way to a problematic platform API, such as an API whose invocation can cause the user's computer to consume excessive amounts of CPU or disk space. This may result in side effects if the browser restricts the functionality of the website, for example, by rejecting attempts to store data in `localStorage` or killing busy scripts.

# Controlling the web message source

If a page handles incoming web messages in an unsafe way, for example, by not verifying the origin of incoming messages correctly in the event listener, properties and functions that are called by the event listener can potentially become sinks. For example, an attacker could host a malicious `iframe` and use the `postMessage()` method to pass web message data to the vulnerable event listener, which then sends the payload to a sink on the parent page. This behavior means that you can use web messages as the source for propagating malicious data to any of those sinks.

## What is the impact of DOM-based web message vulnerabilities?

The potential impact of the vulnerability depends on the destination document's handling of the incoming message. If the destination document trusts the sender not to transmit malicious data in the message, and handles the data in an unsafe way by passing it into a sink, then the joint behavior of the two documents may allow an attacker to compromise the user, for example.

## What is DOM clobbering?

DOM clobbering is a technique in which you inject HTML into a page to manipulate the DOM and ultimately change the behavior of JavaScript on the page. DOM clobbering is particularly useful in cases where XSS is not possible, but you can control some HTML on a page where the attributes `id` or `name` are whitelisted by the HTML filter. The most common form of DOM clobbering uses an anchor element to overwrite a global variable, which is then used by the application in an unsafe way, such as generating a dynamic script URL.

The term clobbering comes from the fact that you are "clobbering" a global variable or property of an object and overwriting it with a DOM node or HTML collection instead. For example, you can use DOM objects to overwrite other JavaScript objects and exploit unsafe names, such as `submit`, to interfere with a form's actual `submit()` function.