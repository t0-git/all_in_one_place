## Command injection

- Code injection should be in the language used by the application. Therefore, the first step in an injection is to find what language is used by the application. To do so you can look at the response's headers, generate errors or look at the way special characters are handle by the application (for example by comparing `+` and `.` for concatenation of strings, or `"` and `'` ...)

```
command1 && command2 that will run command2 if command1 succeeds.
command1 || command2 that will run command2 if command1 fails.
command1 ; command2 that will run command1 then command2.
command1 | command2 that will run command1 and send the output of command1 to command2.
try to use backticks directly : `` or after the first one
try to use $(command2) directly, or after the first one
try url encode, " and ' etc
try a new line (0x0a or \n)

```

- If some characters are blocked, try to encode it (base64 for example) and decode it inside the payload.

- Try with the man ascii to use the # to comment the end of the payload if it's a linux, and then do your tests with etc, passwd, /etc/passwd (maybe some of them are blocked)
