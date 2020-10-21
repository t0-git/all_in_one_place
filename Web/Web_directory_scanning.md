Web_directory_scanning

## ffuf

- ```ffuf -w path/to/wordlist -u https://fuff.io/FUZZ -e .php -c```

### Test parameters

- docker.com/test.php?FUZZ=blabla
- Find the size of the response you want to filter on, for example 24

- add ```-fs 24``` to hide the response with the length of 24

### vhost

- find the length of the non existent vhost :

```curl -s -H "Host: test.forwardslash.htb" http://forwardslash.htb | wc -c``` 

- filter on the size response

- and fuzz ```-H Host:FUZZ.forwardslash.htb -fs <size>```
- or ```Host: FUZZ.htb```
- or even ```Host: FUZZ.FUZZ``` 

### POST request

- Before trying to fuzz it, check the format of the content type, and put the same inside. Generally : ```-X POST -H "Content-Type: application/x-www-form-urlencoded"'```

### Fuzz with error message 

- Example : `Invalid username` in response : add `-fr "Invalid username"`

### Use proxy

- `-x <url_of_the_proxy>`
- I have to check if it works : proxy with credentials : `-x http://<username>:<password>@<url>`


## Access Admin panel by tampering with URI
By @SalahHasoneh1


https://target.com/admin/ –> HTTP 302 (redirect to login page)
https://target.com/admin..;/ –> HTTP 200 OK


https://target.com/../admin
https://target.com/whatever/..;/admin

---

## Bypass 403 Forbidden by tampering with URI

By @remonsec


site.com/secret –> HTTP 403 Forbidden
site.com/secret/ –> HTTP 200 OK
site.com/secret/. –> HTTP 200 OK
site.com//secret// –> HTTP 200 OK
site.com/./secret/.. –> HTTP 200 OK

---

## Bypass Rate limits by adding X- HTTP headers
By @Cyb3rs3curi_ty


Add the following HTTP header(s) with your requests:

X-Originating-IP: IP
X-Forwarded-For: IP
X-Remote-IP: IP
X-Remote-Addr: IP
X-Client-IP: IP
X-Host: IP
X-Forwared-Host: IP
These headers are typically used by intermediate components such as load balancers or proxies and by adding an arbitrary internal IP address in these HTTP headers, we may actually bypass the enforced rate limits.

Try it with IP addresses from the following ranges:

192.168.0.0/16
172.16.0.0/12
127.0.0.0/8
10.0.0.0/8

Once we experience a blockage again, simply increment the provided IP address.