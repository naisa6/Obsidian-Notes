## JWT Vulnerabilities:
- ==JWT authentication bypass via weak signing key==
  https://github.com/wallarm/jwt-secrets/blob/master/jwt.secrets.list
  wget  http://raw.githubusercontent.com/wallarm/jwt-secrets/refs/heads/master/jwt.secrets.list
  On Linux system, run hashcat -a 0 -m 16500 \<jwt> <secret_list>
  This reveals the secret. Generate a new JWT signing key using JWT Editor - New Symmetric Key -> Specify Secret -> Generate and enter base64 encoded secret in the k value. Make sure that the Don't modify header option is selected
  Use this key to sign the JWT after changing the sub claim to administrator
  Access admin panel and delete carlos
- ==JWT authentication bypass via jwk header injection==
  Generate a new RSA key. Send a request containing a JWT to Burp Repeater.  Modify the token's payload - change sub claim to administrator. Click Attack -> Embedded JWK and select the RSA key.
  Send the request to GET /admin  endpoint and delete carlos
- ==JWT authentication bypass via jku header injection==
  In the JWT token header, add a new jku value like: "jku": "https://exploit_id.exploit-server.net/exploit"
  Use RSA key,  if there is none, generate one. Get the kid and replace the token header kid with it. Copy Public key as JWK and paste it in the key set in exploit server body: 
  { "keys": [ ] }
  Change sub claim to administrator.  Sign and don't modify header.   Access the admin panel and delete carlos
- ==JWT authentication bypass via kid header path traversal==
  Generate a new Symmetric key and change the k value to AA== (which is the null byte Base64-encoded).
  
  Change the kid value to ../../../../../../../dev/null and sub claim to administrator
  Sign using the null byte key and Don't modify header Delete carlos