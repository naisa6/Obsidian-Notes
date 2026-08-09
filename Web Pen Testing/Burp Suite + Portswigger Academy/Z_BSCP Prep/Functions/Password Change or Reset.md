## Business Logic Vulnerabilities
- ==Weak isolation on dual-use endpoint==
  Change password and send POST /my-account/change-password to Repeater.
  Change username to administrator and remove current-password parameter, allowing password change.
  Log in as administrator and delete carlos

## Authentication Vulnerabilities
- ==Password reset poisoning via middleware==
  Modify POST request and add a new header and change username parameter to carlos 
  X-Forwarded-Host: exploit-lab_id.exploit-server.net
  View access log and get the user's token and replace it in the URL https://lab_id.web-security-academy.net/forgot-password?temp-forgot-password-token=token
  
  Reset password and log in
- ==Password brute-force via password change==
  Test the password reset function and find that if the passwords do not match, the message shows "New passwords do not match." even if the current password is entered incorrectly.
  Use Intruder to iterate through candidate passwords using Grep-Match with the phrase.

## Race Condition Vulnerabilities
- ==Exploiting time-sensitive vulnerabilities==
  Test the POST /forgot-password endpoint and see that there is a different token every time. Consider the following:
  The token is of a consistent length. This suggests that it's either a randomly generated string with a fixed number of characters, or could be a hash of some unknown data, which may be predictable.
  The fact that the token is different each time indicates that, if it is in fact a hash digest, it must contain some kind of internal state, such as an RNG, a counter, or a timestamp.
  Duplicate the tab and send in parallel, see that there is a different response time, and a different token.
  Send GET /forgot-password request without a Cookie value to obtain a new one, copy the cookie and csrf token and replace those in one of the requests.
  Sending in parallel now should give same response time and same codes.
  Change one of the request's username to carlos. 
  Send in parallel again, one will be sent to carlos and one to wiener, but with the same token. Open the reset token but change the username parameter to carlos and change the password.
  Log in as carlos and delete carlos

## API Testing Vulnerabilities
- ==Exploiting server-side parameter pollution in a query string==
  Test the forgot-password endpoint by typing administrator# The response will show field not specified.Try username=administrator%26field=x%23 There will be an invalid field error. Try username=administrator%26field=§x§%23 in Intruder with Payloads -> Add from list -> Server-side variable names
  review the /static/js/forgotPassword.js JavaScript file. Notice the password reset endpoint, which refers to the reset_token parameter:
  /forgot-password?reset_token=${resetToken}
  Try this in Repeater for POST /forgot-password request:
  username=administrator%26field=reset_token%23
  This will return the reset token
  Access the /forgot-password?reset_token=token   endpoint using the acquired token and change the password to login as administrator to delete carlos