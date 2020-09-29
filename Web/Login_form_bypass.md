Login_form_bypass

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


## SQLMap (USE AT YOUR OWN RISK !)

- Intercept your request with burpsuite and then run sqlmap on it : ```sqlmap -level=5 -risk=3 -p username -r \<yourfile\>``` 
- If you manual tested it before and it should work (because you test for example admin and have an input, and admin' -- and the same input) add ```--string='\<message appeared\>'```
- As always, if sqlmap from the repo doesn't work with ```-r```, download from Git.

---

## WFuzz to enumerate the usernames when you detect a specific message  

- ```wfuzz -c -z file,SecLists/Usernames/Names/names.txt --hs "Try again" -d "username=FUZZ&password=anything" http://10.10.10.73/login.php```

---

## What can it be ?

- MySQL
- MSSQL
- NoSQL
- XPATH

For each one, refer to the cheat sheet.

