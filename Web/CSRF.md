CSRF

## Theory

- When an attacker tricks a victim going to a page controlled by the attacker, which then submits data to the target site as the victim.
- Mitigation with CSRF tokens.
- Check, if they have csrf.js file that sends code like this : $csrf = session CSRF token. On each page, they had script src= csrf.js. So all you have to do was to include that same tag in your exploit.
