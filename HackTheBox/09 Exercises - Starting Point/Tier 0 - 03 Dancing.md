sudo -sV nmap \[host]
sudo apt-get smbclient
smbclient \[host]
[[smbclient]] \\\\\\\\\[host]\\\\Workshares

![[Pasted image 20260223094713.png]]


ls : listing contents of the directories within the share 
cd : changing current directories within the share 
get : downloading the contents of the directories within the share 
exit : exiting the smb shell

ls
cd James.P
get flag.txt
exit
cat flag.txt