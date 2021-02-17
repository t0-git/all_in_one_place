Shodan dorks

## RocketMQ

 org:target.com http.title:rocketmq-console

## Spring boot

https://twitter.com/sw33tLie/status/1276266817053392900

Search for the following favicon hash in Shodan to find Spring Boot servers deployed in the target organization:

org:YOUR_TARGET http.favicon.hash:116323821
Then check for exposed actuators. If /env is available, you can probably achieve RCE. If /heapdump is accessible, you may find private keys and tokens.

In case you are unfamiliar with Spring Boot technology, do not worry. Here’s a quick 101.

Spring Boot is an open source Java-based framework used to build stand-alone spring applications based on the concepts of micro services.

Spring Boot Actuator is a mechanism of interacting with them using a web interface. They are typically mapped to URL such as:

https://target.com/env
https://target.com/heapdump
etc.


Pro tip: Check for all these default built-in actuators : https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#production-ready-endpoints-exposing-endpoints. Some of them may be exposed and contain interesting information.

## FTP related and credentials

“default password” org:orgName
“230 login successful” port:21 org:orgName
vsftpd 2.3.4 port:21 org:orgName
230 ‘anonymous@’ login ok org:orgName
guest login ok org:orgName
country:EU port 21 -530 +230 org:orgName
country:IN port:80 title:protected org:orgName

## Web servers on non standard ports

HTTP ASN:<here> -port:80,443,8080

lookup for ASNs with amass