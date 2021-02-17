JWT

- Install Json Web Tokens extensions in burp.

## Theory

- Used to authenticate
- Composed of three parts separated by dots (base64 encoded)
1. Header usually contains typ (type of the token), and alg (algorithm used to sign the token)
2. Payload contains a lot of information (mail address, username, userid ...)
3. Signature : The signature is used to verify the message wasn't changed along the way, and, in the case of tokens signed with a private key, it can also verify that the sender of the JWT is who it says it is. To create the signature part you have to take the encoded header, the encoded payload, a secret, the algorithm specified in the header, and sign that.

For example if you want to use the HMAC SHA256 algorithm, the signature will be created in the following way:
```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

- A JWT typically looks like the following.

```xxxxx.yyyyy.zzzzz``` base64 encoded.

- JWT can provide the following mechanism : Encryptions / Signature.

### Different kind of signatures

RSA based
Elliptic curves
HMAC
None

## Kind of attacks

- Use decoder in burp, easy and pain free. Pay attention, it could miss one `=` or 2.

### None algorithm attack

- Application support the none algorith, signature is useless.
- Capture the JWT.
- Change the algorithm to None.
- Change the content of the claims in the body with whatever you want e.g.: email: attacker@gmail.com
- Send the request with the modified token and check the result.

- If it doesn't work, try to remove the signature.



### Changing algorithm

- If the algorithm is RS256 change to HS256 and sign the token with the public key (which you can get by visiting jwks Uri / mostly it will be the public key from the site’s https certificate)
- Send the request with the modified token and check the response.


### Signature not checked

- Just remove the signature.

### Install jwt heartbreaker on burp.

### Brute force key

jwtbrute
jwt cracker

### Check for proper server-side session termination (OTG-SESS-006):

- Check if the application is using JWT tokens for authentication.
- If so, login to the application and capture the token. (Mostly web apps stores the token in the local storage of the browser)
- Now logout of the application.
- Now make a request to the privileged endpoint with the token captured earlier.
- Sometimes, the request will be successful as the web apps just delete the token from browser and won’t blacklist the tokens in the backend.

## Online JWT debugger

https://jwt.io/#debugger-io

## Roadmap

![bf63cc72584b01b0bd61cd15b6dffda4.png](../../_resources/b85e88dd7dd34d669a103a58419edc06.png)



---

https://jwt.io/introduction/