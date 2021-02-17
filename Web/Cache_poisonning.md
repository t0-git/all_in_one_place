Cache_poisonning

## Theory 

When the cache received an HTTP request, it will determine if there is a cached reponse. If so, it will use it and will don't forward to the back end server.

## How to

As Web Cache Poisoning relies on manipulation of unkeyed input, you can use param miner to know if you can use one. 

Then, understand how the website processes it.

If it's not sanitized and reflected on the website, you can use it to inject your payload.

## Example

- Right click on your request in burp, guess headers, then go to extender / extensions / param miner / output.

- With a double Host header, the second one is reflected in the web page and try to load a `/src/file.js`

- Create your malicious js file on your webserver and send the request with your webserver as the second host.