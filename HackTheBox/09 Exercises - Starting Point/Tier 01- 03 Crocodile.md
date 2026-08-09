[[ping]] the host
sudo nmap -sC -sV -p- --min-rate=1000 \[host]
[[ftp]] 10.129.1.15
Name (10.129.1.15:root): anonymous
dir
get allowed.userlist
get allowed.userlist.passwd

gobuster dir --url http://[host]/ --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x php,html

http://[host]/login.php

