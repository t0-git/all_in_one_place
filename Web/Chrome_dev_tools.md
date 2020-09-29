Chrome_dev_tools

- Start by right clicking, inspect (or CTRL + SHIFT + I)

---

## Sources

- You have the tree structure of the current page 

---

## Network

- Reload the page, check the url you are visiting, and you will see the request and response

### JS files

- First of all : Refresh
**Beautify it:**
- Right click on a javascript file, open in sources panel
- Click on the bracket to have something beautified ("pretty print")
![fef3c42e6497e5934c15f16ac8df83d6.png](../../_resources/7629688902864efbb214a9d8e58a1741.png)

**XHR:**  
- Fetching remote files with javascript (XMLHttpRequest)
- If you click on an initiator in XHR, you'll be redirected on the line of the code which is responsible of this request
- Try to search for  ```/api, /v1, /yql``` ...

![a8fa8e3c9c016c3fe6e6011df476c040.png](../../_resources/61ddb58cf8284507ac8966efb11fed6f.png)

- Open Dev Tools
- In Sources > Search the little menu ```Event Listeners```

![7e23a1f70e310d7eb8e5e247a21ecd17.png](../../_resources/91314bd4cc784e58b44856ab85c117f5.png)

- Search the event you want to trigger and try to put some breakpoints in the code to see how it works.

---

## Console

- Execute javascript code here when you can.

