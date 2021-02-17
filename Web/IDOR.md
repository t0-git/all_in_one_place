## Theory

- Insecure Direct Object Reference 
- Happens when access control is not properly implemented, and when references to data objects are predictable.
- Example : https://url...id=1
- Try to change the id and see what happens.
- You can : read informations, edit data, send XSS ...
- Location : URL parameters, form field parameters, file paths, headers, cookies.

## Tips

### Hashes

- If something is encoded, try to decode it using common encoding schemes.
- If it's randomized, see if it's predictable. So create a few accounts to analyze how the hash is created to find a pattern.
- May use API to leak random or hashed IDs, or other public pages in the application, or in a URL via referer.

### Parameters

- In an API URL, try to change the key for example ```bucket?price=<id>```, to ```bucket?user_id=<another_id``` may lead to a list or something else.
- Try adding id to the request, even if it's not in your initial one, for example : ```GET /api/messages``` , to ```GET /api/messages?id=<user_id>```

### HPP (HTTP parameter pollution)

- Applications might not anticipate the user submitting multiple values for the same parameter. Examples :
```GET /api_v1/messages?user_id=ANOTHER_USERS_ID```

To try : 
```
GET /api_v1/messages?user_id=YOUR_USER_ID&user_id=ANOTHER_USERS_ID
GET /api_v1/messages?user_id=ANOTHER_USERS_ID&user_id=YOUR_USER_ID
GET /api_v1/messages?user_ids[]=YOUR_USER_ID&user_ids[]=ANOTHER_USERS_ID
```

### Change request method

- Common trick : POST for PUT or vice versa. Access controls might not have been implemented the same way. 
- Try also with the others (DELETE, PATCH...)

### Change requested type file

- Add .json to the end of the request URL

### Recon for IDOR:

- Try to search engine scrape for UUIDs, ex: google dork for IDOR URL parameters

- Using tools like WaybackURLS or gau, and grep for UUID's, IDs and common IDOR URL parameters

- Scraping JS files for API endpoints with UUID's, common IDOR parameters


**Every time you see a new API endpoint that receives an object ID from the client:**

- Does the ID belong to a private resource? (e.g /api/user/123/news vs /api/user/123/transaction)

- What are the IDs that belong to me?

- What are the different possible roles in the API?(For example — user, driver, supervisor, manager)

### Bypassing Object Level Authorization:

- Add parameters onto the endpoints for example:

```
GET /api\_v1/messages --> 401 
vs 
GET /api\_v1/messages?user\_id=victim\_uuid --> 200
```


- To find URL parameters for endpoints, use tools like [Arjun](https://github.com/s0md3v/Arjun) to bruteforce common IDOR URL parameter names.

### HTTP Parameter pollution

```
GET /api\_v1/messages?user\_id=VICTIM\_ID --> 401 Unauthorized 
vs
GET /api\_v1/messages?user\_id=ATTACKER\_ID&user\_id=VICTIM\_ID --> 200 OK GET 
or
/api\_v1/messages?user\_id=YOUR\_USER\_ID\[\]&user\_id=ANOTHER\_USERS_ID\[\]
```

### Test on outdated API Versions


```
/v3/users\_data/1234 --> 403 Forbidden
vs
/v1/users\_data/1234 --> 200 OK
```

### Wrap the ID with an array.

```
{“id”:111} --> 401 Unauthorized
vs
{“id”:\[111\]} --> 200 OK
```

### Wrap the ID with a JSON object:

```
{“id”:111} --> 401 Unauthorized
vs
{“id”:{“id”:111}} --> 200 OK
```

### JSON Parameter Pollution:

```
POST /api/get\_profile Content-Type: application/json {“user\_id”:<legit_id>,”user_id”:<victim’s_id>}
```

### MFLAC:

```
GET /admin/profile --> 401 Unauthorized 
GET /ADMIN/profile --> 200 OK
```


### Random Tips/Tricks:

- Try to send a wildcard(*) instead of an ID. It’s rare, but sometimes it works.

- Check through the same corresponding mobile API endpoints for the webapp, to find uuids, if you need them to complete the IDOR exploitation

- Many times there will be endpoints to translate emails into GUID's, check for those

- If it is a number id, be sure to test through a large amount of numbers, instead of just guessing Ex: Burp intruder from ID 100-1000

- If endpoint has a name like /api/users/myinfo, check for /api/admins/myinfo

- Replace request method with GET/POST/PUT


- Escalating/Chaining with IDOR's Ideas:

1. Lets say you find a low impact IDOR, like changing someone elses name, chain that with XSS and you have stored XSS!

2. If you find IDOR on and endpoint, but it requires UUID, chain with info disclosure endpoints that leak UUID, and bypass this!


---

## How to increase the impact of IDOR

### Critical IDORs first

- Always look for IDORs in critical functionalities first. Both write and read based IDORs can be of high impact.

- In terms of state-changing (write) IDORs, password reset, password change, account recovery IDORs often have the highest business impact. (Say, as compared to a “change email subscription settings” IDOR.)

- As for non-state-changing (read) IDORs, look for functionalities that handle the sensitive information in the application. For example, look for functionalities that handle direct messages, sensitive user information, and private content. Consider which functionalities on the application makes use of this information and look for IDORs accordingly.


### Stored-XSSPermalink

- When you combine write-IDOR with self-XSS, you can often create a stored-XSS targeted towards a specific user.

- When would this be useful? Let’s say you find an IDOR that allows attackers to change the content of another user’s internet shopping list. This IDOR in itself would not be too high impact, and would likely just cause a bit of annoyance if exploited in the wild. But if you can chain this IDOR with a self-XSS on the same input field, you can essentially use this IDOR to deliver the XSS exploit code to the victim user’s browser. This way, you can create targeted stored-XSS that requires no user interaction!
- 



@Jhaddix - [https://www.youtube.com/watch?v=94-tlOCApOc](https://www.youtube.com/watch?v=94-tlOCApOc)

@vickieli7 - [https://medium.com/@vickieli/how-to-find-more-idors-ae2db67c9489](https://medium.com/@vickieli/how-to-find-more-idors-ae2db67c9489)

@inonst - [https://medium.com/@inonst/a-deep-dive-on-the-most-critical-api-vulnerability-bola-1342224ec3f2](https://medium.com/@inonst/a-deep-dive-on-the-most-critical-api-vulnerability-bola-1342224ec3f2)










