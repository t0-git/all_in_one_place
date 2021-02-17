## List of CSPs : 

```
script-src : This directive specifies allowed sources for JavaScript. This includes not only URLs loaded directly into <script> elements, but also things like inline script event handlers (onclick) and XSLT stylesheets which can trigger script execution.
	
default-src: This directive defines the policy for fetching resources by default. When fetch directives are absent in CSP header the browser follows this directive by default.
Child-src: This directive defines allowed resources for web workers and embedded frame contents.

connect-src: This directive restricts URLs to load using interfaces like <a>,fetch,websocket,XMLHttpRequest
frame-src: This directive restricts URLs to which frames can be called out.

frame-ancestors: This directive specifies the sources that can embed the current page. This directive applies to <frame>, <iframe>, <embed>, and <applet> tags. This directive can't be used in <meta> tags and applies only to non-HTML resources.
img-src: It defines allowed sources to load images on the web page.

Manifest-src: This directive defines allowed sources of application manifest files.

media-src: It defines allowed sources from where media objects like <audio>,<video> and <track> can be loaded.

object-src: It defines allowed sources for the <object>,<embed> and <applet> elements.

base-uri: It defines allowed URLs which can be loaded using <base> element.

form-action: This directive lists valid endpoints for submission from <form> tags.

plugin-types: It defineslimits the kinds of mime types a page may invoke.

upgrade-insecure-requests: This directive instructs browsers to rewrite URL schemes, changing HTTP to HTTPS. This directive can be useful for websites with large numbers of old URL's that need to be rewritten.
	
sandbox: sandbox directive enables a sandbox for the requested resource similar to the <iframe> sandbox attribute. It applies restrictions to a page's actions including preventing popups, preventing the execution of plugins and scripts, and enforcing a same-origin policy.
```

## Sources
Sources are nothing but the defined directives values. Below are some common sources that are used to define the value of the above directives.

```
   * : This allows any URL except data: blob: filesystem: schemes
self : This source defines that loading of resources on the page is  allowed from the same domain.

data: This source allows loading resources via the data scheme (eg Base64 encoded images)

none: This directive allows nothing to be loaded from any source.
unsafe-eval : This allows the use of eval() and similar methods for creating code from strings. This is not a safe practice to include this source in any directive. For the same reason it is named as unsafe. 

unsafe-hashes: This allows to enable specific inline event handlers.

unsafe-inline: This allows the use of inline resources, such as inline <script> elements, javascript: URLs, inline event handlers, and inline <style> elements. Again this is not recommended for security reasons.

nonce: A whitelist for specific inline scripts using a cryptographic nonce (number used once). The server must generate a unique nonce value each time it transmits a policy.
```

CSP evaluators : 

1. https://csp-evaluator.withgoogle.com/
2. https://cspvalidator.org/