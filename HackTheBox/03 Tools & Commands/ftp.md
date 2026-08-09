The File Transfer Protocol (FTP) is a standard communication protocol used to transfer computer files from a server to a client on a computer network. FTP is built on a client– server model architecture using separate control and data connections between the client and the server. FTP users may authenticate themselves with a clear-text sign-in protocol, generally in the form of a username and password. However, they can connect anonymously if the server is configured to allow it. For secure transmission that protects the username and password and encrypts the content, FTP is often secured with SSL/TLS (FTPS) or replaced with SSH File Transfer Protocol (SFTP).


extra services like SSH (Secure Shell Protocol) and HTTPD (Web Server) in order to explore a more typical example. With this type of configuration, a network administrator has set up a rudimentary core web server configuration, allowing them to achieve the following, all at the same time if need be: Receive and send files that can be used to configure the webserver or serve logs to an external source Be able to be logged into for remote management from a distant host, in case any configuration changes are needed Serve web content that can be accessed remotely through another host's web browser

A typical misconfiguration for running FTP services allows an anonymous account to access the service like any other authenticated user. The anonymous username can be input when the prompt appears, followed by any password whatsoever since the service will disregard the password for this specific account.

ftp \[host]

To connect to the remote FTP server, you need to specify the target's IP address (or hostname), as displayed on the Starting Point lab page. The prompt will then ask us for our login credentials, which is where we can fill in the anonymous username. Misconfigured FTP server may not request a password, and inputting the anonymous username may lead to  230 code, Login successful .

## Navigation Commands

ls              # list files in remote directory  
dir             # detailed list (same as ls)  
pwd             # show current remote directory  
cd <folder>     # change remote directory  
lcd <folder>    # change local directory
get filename # download single file  
mget *.txt # download multiple file


FTP server may be configured to allow anonymous login:
ftp-anon: Anonymous FTP login allowed (FTP code 230)