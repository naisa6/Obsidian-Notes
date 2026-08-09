An SMB-enabled storage on the network is called a share . These can be accessed by any client that has the address of the server and the proper credentials. Like many other file access protocols, SMB requires some security layers to function appropriately within a network topology. If SMB allows clients to create, edit, retrieve, and remove files on a share, there is a clear need for an authentication mechanism. At a user level, SMB clients are required to provide a username/password combination to see or interact with the contents of the SMB share.
Despite having the ability to secure access to the share, a network administrator can sometimes make mistakes and accidentaly allow logins without any valid credentials or using either guest accounts or anonymous log-ons . We will witness this in the following sections.

[-L|--list=HOST] : Selecting the targeted host for the connection request.
-L : List available shares on the target/ what services are available on a server
-U : Login identity to use.
-N : No password
