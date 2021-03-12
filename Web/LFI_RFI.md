## LFI STUFF

- Try with and without the %00 at the end. (fixed since php 5.3)

- If it returns nothing because of encoded php files (or you can just use it to have all the output) : `php://filter/convert.base64-encode/resource=\<your ressource for example config.php\>`

- If you have an error message with assert try this : `=%27%20and%20die(show_source(%27.passwd%27))%20or%20%27` , or `'.system("ls -la").'`

- Read it in plain text : `param=php://filter/convert.iconv.utf-8.utf-16/resource=<file>`

- Try all the php streams you think about

- This resource is awesome : http://repository.root-me.org/Exploitation%20-%20Web/EN%20-%20Local%20File%20Inclusion.pdf . So you can poison logs and then access to the logs to execute php, you can find logs in /proc/self. Moreover, it could be url encoded, but the authorization header is not. So put inside base64 code and you have poisoned your logs.

---

## LFI via XSS 

https://www.noob.ninja/2017/11/local-file-read-via-xss-in-dynamically.html

```
Payload :  <script>x=new XMLHttpRequest;x.onload=function(){document.write(this.responseText)};x.open("GET","file:///etc/passwd");x.send();</script>
```

(book box on HackTheBox)


---

## Practical exercise

https://medium.com/@asfiyashaikh10/file-path-traversal-and-file-inclusions-7c567da9e226

---

## HOW TO FUZZ TO FIND LOGS :

ZAProxy fuzz with Fuzz/LFI file, as burp community has a rate limit

---

## LFI Cheat sheet :

https://highon.coffee/blog/lfi-cheat-sheet/

---

## LFI to RCE

- Have to search for RCE vector :

1. Using /proc/self/environ

2. Using /proc/self/fd

3. Using log files with controllable input like:

    - /var/log/apache2/access.log

    - /var/log/apache2/error.log

    - ... (depends, OS, webserver etc...)