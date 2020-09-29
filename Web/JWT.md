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

```xxxxx.yyyyy.zzzzz```

## Kind of attacks

### None algorithm attack

- Application support the none algorith, signature is useless.
- Decode the token
- Change whatever you want inside the jwt

### Changing algorithm

- Example change RS256 : assymetric to HS256 : symetric, so you can use the public key to sign and verify

### Signature not checked

- Just remove the signature.

### Brute force key

jwtbrute
jwt cracker

## Online JWT debugger

https://jwt.io/#debugger-io

---

https://jwt.io/introduction/