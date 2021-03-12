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

Catch the public key, then :
- script in python to do that.
- or use jwt.io to have the first two parts of your jwtoken, then

```
cat pub.key | xxd -p | tr -d "\\n"

echo -n "<jwt_without_signature>" | openssl dgst -sha256 -mac HMAC -macopt hexkey:<pubkey_in_hex>

python -c "exec(\"import base64,binascii;print(base64.urlsafe_b64encode(binascii.a2b_hex(b'496ed19de73419b6caaa5df35cb56ba88a9565b482338f7fcd60a462f301c239')))\")"

```

eventually remove the = at the end.

put the signature at the end of the first two parts

### Signature not checked

- Just remove the signature.

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

![bf63cc72584b01b0bd61cd15b6dffda4.png](../_resources/b85e88dd7dd34d669a103a58419edc06.png)


## Tool for JWT

### jwt-tool

- Change signature to None : ``-X a`
- Modify username field by admin  : `jwt-tool <jwt_token> -I -pc username -pv admin`
- Bruteforce the secret on symetrical encryption (HS512...): `jwt-tool <jwt_token> -C -d <dictionnary_file>`
- Modify it with the good secret : `jwt-tool <jwt_token> -I -pc role -pc admin -S hs512 -p <secret>`
- jwt with a public key : `jwt-tool eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6InRlc3QifQ.Y6AeSWAcgxAUyPkxSKEajm8NxKZFHlJqOPp62JPio0NiamTmTzc-YEKt3ZyguLjf5yQcTSPW1WNjBnW-O3OPnHRi2z2rOoCbvx-RYSqXWq0cJsB0Ig6ICdWQtFCW7vEjTccQCcL6vGqw1BlimhfwvuuKFhziyGNDKs0ut22eg21xyY0ny3wZ1meMsBgmgYCzcWO5r5s0ICeYeeaBz__JRcQ42k7-WbdHkQg488_nV3hm_omG1oIt751Yp-HZPphm-hi8YTJfjDWEBkz5ooQ3giwKK4MMJnDPxiJqn378UYnmlYvF1yGTXzm5FjRH12rzyAQcamOoWdA-l1Ol3GYIMg -V -pk /home/t0/key.rsa`

### python

- Use python-pyjwt

---

## Try to fuzz the end of the jwt adding = or == (or \r or \n encoded in b64)

https://jwt.io/introduction/


