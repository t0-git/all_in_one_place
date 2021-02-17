## Have to work on

## LFI STUFF

- Try with and without the %00 at the end.

- If it returns nothing because of encoded php files : php://filter/convert.base64-encode/resource=\<your ressource for example config.php\>

---

## LFI in PDF upload 

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

ZAProxy fuzz with Fuzz/LFI file

---

## Test LFI :

https://0xdf.gitlab.io/2019/07/13/htb-friendzone.html

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