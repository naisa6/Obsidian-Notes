## Which sinks can lead to DOM-based open-redirection vulnerabilities?

The following are some of the main sinks can lead to DOM-based open-redirection vulnerabilities:

`location location.host location.hostname location.href location.pathname location.search location.protocol location.assign() location.replace() open() element.srcdoc XMLHttpRequest.open() XMLHttpRequest.send() jQuery.ajax() $.ajax()`

## Which sinks can lead to DOM-based cookie-manipulation vulnerabilities?

The `document.cookie` sink can lead to DOM-based cookie-manipulation vulnerabilities.

## Which sinks can lead to DOM-based JavaScript-injection vulnerabilities?

The following are some of the main sinks that can lead to DOM-based JavaScript-injection vulnerabilities:

`eval() Function() setTimeout() setInterval() setImmediate() execCommand() execScript() msSetImmediate() range.createContextualFragment() crypto.generateCRMFRequest()`

## Which sinks can lead to DOM-based document-domain manipulation vulnerabilities?

The `document.domain` sink can lead to DOM-based document-domain manipulation vulnerabilities.

## Which sinks can lead to WebSocket-URL poisoning vulnerabilities?

The `WebSocket` constructor can lead to WebSocket-URL poisoning vulnerabilities.

## Which sinks can lead to DOM-based link-manipulation vulnerabilities?

The following are some of the main sinks can lead to DOM-based link-manipulation vulnerabilities:

`element.href element.src element.action`

## Which sinks can lead to DOM-based web-message manipulation vulnerabilities?

The `postMessage()` method for sending web messages can lead to vulnerabilities if the event listener for receiving messages handles the incoming data in an unsafe way.

## Which sinks can lead to DOM-based Ajax request-header manipulation vulnerabilities?

The following are some of the main sinks can lead to DOM-based Ajax request-header vulnerabilities:

`XMLHttpRequest.setRequestHeader() XMLHttpRequest.open() XMLHttpRequest.send() jQuery.globalEval() $.globalEval()`

## Which sinks can lead to DOM-based local file-path manipulation vulnerabilities?

The following are some of the main sinks can lead to DOM-based local file path manipulation vulnerabilities:

`FileReader.readAsArrayBuffer() FileReader.readAsBinaryString() FileReader.readAsDataURL() FileReader.readAsText() FileReader.readAsFile() FileReader.root.getFile()`

## Which sinks can lead to DOM-based client-side SQL-injection vulnerabilities?

The JavaScript database function `executeSql()` can lead to client-side SQL-injection vulnerabilities.

## Which sinks can lead to DOM-based HTML5-storage manipulation vulnerabilities?

The following are some of the main sinks that can lead to DOM-based HTML5-storage manipulation vulnerabilities:

`sessionStorage.setItem() localStorage.setItem()`

## Which sinks can lead to XPath-injection vulnerabilities?

The following are some of the main sinks that can lead to DOM-based XPath-injection vulnerabilities:

`document.evaluate() element.evaluate()`

## Which sinks can lead to DOM-based JSON-injection vulnerabilities?

`JSON.parse() jQuery.parseJSON() $.parseJSON()`

## Which sinks can lead to DOM-data manipulation vulnerabilities?

The following are some of the main sinks that can lead to DOM-data manipulation vulnerabilities:

`script.src script.text script.textContent script.innerText element.setAttribute() element.search element.text element.textContent element.innerText element.outerText element.value element.name element.target element.method element.type element.backgroundImage element.cssText element.codebase document.title document.implementation.createHTMLDocument() history.pushState() history.replaceState()`

## Which sinks can lead to DOM-based denial-of-service vulnerabilities?

The following are some of the main sinks can lead to DOM-based denial-of-service vulnerabilities:

`requestFileSystem() RegExp()`

## Which sinks can lead to DOM-based web message vulnerabilities?

As long as a website accepts web message data from an untrusted source due to a lack of adequate origin verification, any sinks that are used by the incoming message event listener could potentially lead to vulnerabilities.