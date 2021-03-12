## Find the mechanism used

### By blocks (ECB)

- register an account with 30 a and 30 b.
- Then look at your cookie in hex, decode it if needed before (URL and base64).
- Try to find a pattern, is there any delimitter ? like pattern1/delimiter/pattern2
- Then find the order, is it username / delimiter / password, or password / delimiter / username ? (for this, use a long username and a short password)
- Find after that the size of the delimiter (what's the size of the entire payload with a username of length 2, password with length 2, then 3 / 2, then 3 / 3 etc)
- If you found block of 8, try 8 a, admin after that to view how it's encoded. 
- remove the junk bytes (8 a), encode the payload and put your cookie back

