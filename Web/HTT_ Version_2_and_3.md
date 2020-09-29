HTT_ Version_2_and_3

## HTTP2

- after 1.1, 2 : compatibility with 1.1
- loading faster thanks to headers http compressed, new mecanism "push" which permits to send data to the client even if he doesn't ask it, requests pipelined, multiplexed requests
- take in charge new usages like tablets, mobiles, API, web servers etc

---

## HTTP3

- Completely rewritted the http protocol, using QUIC (Quick UDP Internet Connection) protocol instead of TCP, with integrated support of TLS. All of this used to loads websites faster and have crypted connection by default.

- QUIC merges all the best functionnalities from TCP and UDP, to create a layer 4 protocol faster than ever.

- In NMAP scan, most of the time, you can see it with UDP port scan, 443 is open | filtered.

- Support of HTTP3 with : ```quiche```
- or use ```curl-http3``` in a docker image which is compiled with boringSSL and quiche/0.2.0 for http3 support :
```
git clone https://github.com/yurymuski/curl-http3.git
sudo docker run --net host --privileged -it --rm ymuski/curl-http3 /bin/bash 
curl --http3 \<url\>
```
- download a file : ```-o <output_file>```
- copy it inside your host : 
```
sudo docker ps
sudo docker cp \<image name\>:/\<path_in_the_container\> \<path_to_your_host\>
```