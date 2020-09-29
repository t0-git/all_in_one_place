Same-Origin_Policy

## Theory

- How the browser restricts a number of security critical features :
1. What domains you can contact via XMLHttpRequest
2. Access to the DOM accross separate frames/windows

- Much more strict than cookies :
1. Protocol must match ; no crossing HTTP/HTTPS
2. Port numbers must match
3. Domain names must be an exact match
