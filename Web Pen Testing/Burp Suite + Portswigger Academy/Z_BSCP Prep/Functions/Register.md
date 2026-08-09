## Business Logic Vulnerabilities
- ==Inconsistent handling of exceptional input==
  Test the email parameter. Registering with a very long string truncates the email address to 255 characters, although it will send the verification code to the full email address.
  Make the email end in @dontwannacry.com.exploit_id.exploit-server.net, but the total length of the email up to the dontwannacry.com part should be 255 characters. This ensures that a verification email is sent but the displayed email is dontwannacry.com, which grants admin access
- 