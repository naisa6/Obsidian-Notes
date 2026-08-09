## What is the impact of DOM-based open redirection?

This behavior can be leveraged to facilitate phishing attacks against users of the website, for example. The ability to use an authentic application URL targeting the correct domain and with a valid TLS certificate (if TLS is used) lends credibility to the phishing attack because many users, even if they verify these features, will not notice the subsequent redirection to a different domain.

If an attacker is able to control the start of the string that is passed to the redirection API, then it may be possible to escalate this vulnerability into a JavaScript injection attack. An attacker could construct a URL with the `javascript:` pseudo-protocol to execute arbitrary code when the URL is processed by the browser.

## What is the impact of a DOM-based cookie-manipulation attack?

The potential impact of this vulnerability depends on the role that the cookie plays within the website. If the cookie is used to control the behavior that results from certain user actions (for example, a production versus demo mode setting), then the attacker may be able to cause the user to perform unintended actions by manipulating the cookie's value.

If the cookie is used to track the user's session, then the attacker may be able to perform a session fixation attack, in which they set the cookie's value to a valid token that they have obtained from the website, and then hijack the session during the victim's subsequent interaction with the website. A cookie-manipulation vulnerability like this can be used to attack not only the vulnerable website, but any other website under the same parent domain.

## What is the impact of a DOM-based JavaScript-injection attack?

The attacker-supplied code can perform a wide variety of actions, such as stealing the victim's session token or login credentials, performing arbitrary actions on the victim's behalf, or even logging their keystrokes.

## What is the impact of WebSocket-URL poisoning?

The potential impact of WebSocket-URL poisoning depends on how the website uses WebSockets. If the website transmits sensitive data from the user's browser to the WebSocket server, then the attacker may be able to capture this data.

If the application reads data from the WebSocket server and processes it in some way, the attacker may be able to subvert the website's logic or deliver client-side attacks against the user.

## What is the impact of a DOM-based link-manipulation attack?

An attacker may be able to leverage this vulnerability to perform various attacks, including:

- Causing the user to be redirected to an arbitrary external URL, which could facilitate a phishing attack.
- Causing the user to submit sensitive form data to a server controlled by the attacker.
- Changing the file or query string associated with a link, causing the user to perform an unintended action within the application.
- Bypassing browser anti-XSS defenses by injecting on-site links containing XSS exploits. This works because anti-XSS defenses do not typically account for on-site links.
## What is the impact of DOM-based Ajax request-header manipulation?

The potential impact of the vulnerability depends on the role of specific HTTP headers in the server-side processing of the Ajax request. If the header is used to control the behavior that results from the Ajax request, the attacker may be able to cause the user to perform unintended actions by manipulating the header. The impact also depends on what exactly the attacker is able to inject into the headers.

## What is the impact of DOM-based local file-path manipulation?

The potential impact of this vulnerability depends on how the website uses the opened file:

- If the website reads data from the file, the attacker may be able to retrieve this data.
- If the website writes specific data to a sensitive file, the attacker may also be able write their own data to the file, which could be the configuration file of the operating system, for example.

In both of these cases, the actual exploitability of the potential vulnerability may depend on other suitable functionality being present on the website.

## What is the impact of DOM-based client-side SQL injection?

The potential impact of the vulnerability depends on the website's usage of the SQL database. If the database is used to store sensitive data, such as messages on a social network, the attacker may be able to retrieve this data.

If the database is used to store pending user actions, such as outgoing messages in an email application, then the attacker may be able to modify this data and perform arbitrary actions on the user's behalf.

## What is the impact of DOM-based XPath injection?

Depending on the purpose for which the query results are used, it may be possible for the attacker to subvert the website's logic or cause unintended actions on behalf of the user.

## What is the impact of a DOM-based JSON-injection attack?

Depending on the purpose for which this data is used, it may be possible for an attacker to subvert the website's logic, or cause unintended actions on behalf of another user.

## What is the impact of DOM-data manipulation?

At the lesser end of the scale, an attacker may be able to leverage this vulnerability to perform virtual defacement of the website, such as changing text or images that are displayed on a particular page. However, attacks can be more severe. For example, if the attacker is able to change the `src` property of an element, they could potentially induce the user to perform unintended actions by importing a malicious JavaScript file.

## What is DOM-based denial of service?

DOM-based denial-of-service vulnerabilities arise when a script passes attacker-controllable data in an unsafe way to a problematic platform API, such as an API whose invocation can cause the user's computer to consume excessive amounts of CPU or disk space. This may result in side effects if the browser restricts the functionality of the website, for example, by rejecting attempts to store data in `localStorage` or killing busy scripts.

## What is the impact of DOM-based web message vulnerabilities?

The potential impact of the vulnerability depends on the destination document's handling of the incoming message. If the destination document trusts the sender not to transmit malicious data in the message, and handles the data in an unsafe way by passing it into a sink, then the joint behavior of the two documents may allow an attacker to compromise the user, for example.