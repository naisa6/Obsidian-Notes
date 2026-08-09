Impacket is a framework written in Python for working with network protocols. It is focused on providing low-level programmatic access to the packets and for some protocols (e.g. SMB and MSRPC) the protocol implementation itself. In short, Impacket contains dozens of amazing tools for interacting with windows systems and applications, many of which are ideal for attacking Windows and Active Directory.

One of the most commonly used tools in impacket is psexec.py . It is named after the utility, PsExec from Microsoft’s Sysinternals suite since it performs the same function of enabling us to execute a fully interactive shell on remote Windows machines.

Impacket tool includes a valuable python script called mssqlclient.py which offers such functionality.
-windows-auth : this flag is specified to use Windows Authentication
e.g. python3 mssqlclient.py ARCHETYPE/sql_svc@{TARGET_IP} -windows-auth

