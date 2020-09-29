Content_sniffing

## Theory

- Send data to the browser without sending information of what you are sending.

## MIME Sniffing

- Browser will often not just loo at the Content-Type header that the server is passing, but also the contents of the page. If it looks enough like HTML, it will be parsed as HTML. 

## Encoding sniffing

- Encoding used on a document will be sniffed by (old) browsers. If you are able to control the way the browser decodes text, you may be able to alter after the parsing.