# XML / XXE

## XPath

The code can use the user's input, inside an XPath expression. XPath is a query language, which selects nodes from an XML document. Imagine the XML document as a database, and XPath as an SQL query. If you can manipulate the query, you will be able to retrieve elements to which you normally should not have access.

If we inject a single quote, we can see the following error:

Warning: SimpleXMLElement::xpath(): Invalid predicate in /var/www/index.php on line 22
Warning: SimpleXMLElement::xpath(): xmlXPathEval: evaluation failed in /var/www/index.php on line 22
Warning: Variable passed to each() is not an array or object in /var/www/index.php on line 23
Just like SQL injection, XPath allows you to do boolean logic, and you can try:

' and '1'='1 and you should get the same result.
' or '1'='0 and you should get the same result.
' and '1'='0 and you should not get any result.
' or '1'='1 and you should get all results.
Based on these tests and previous knowledge of XPath, it's possible to get an idea of what the XPath expression looks like:

[PARENT NODES]/name[.='[INPUT]']/[CHILD NODES]
To comment out the rest of the XPath expression, you can use a NULL BYTE (which you will need to encode as %00). As we can see in the XPath expression above, we also need to add a ] to properly complete the syntax. Our payload now looks like hacker']%00 (or hacker' or 1=1]%00 if we want all results).

If we try to find the child of the current node, using the payload '%20or%201=1]/child::node()%00, we don't get much information.

Here, the problem is that we need to get back up in the node hierarchy, to get more information. In XPath, this can be done using parent::* as part of the payload. We can now select the parent of the current node, and display all the child node using hacker'%20or%201=1]/parent::*/child::node()%00

One of the node's value looks like a password. We can confirm this, by checking if the node's name is password using the payload hacker']/parent::*/password%00.

## Tips XXE

Payload all the things

- Double URL encode your payload 

- find the key in the code if needed 

---




