1. Always check if the **robots.txt** has any sensitive paths

2.  Add new parameters and check if applicaiton uses it for access control
```json
{
	"email":"someemail@email.com"
}
```
Just adding the value _roleid:2_ gives us access to admin panel
```json
{
	"email":"someemail@email.com",
	"roleid":2
}
```

3. If application URL based access control then try using different headers
```text
X-Original-URL: /admin/deleteUser
X-Rewrite-URL: /admin/deleteUser
```

4. Change the HTTP request method
```text
GET POST PUT PATCH DELETE TRACE OPTIONS HEAD CONNECT
```

5. Always look for **reset tokens** when doing **[[Wayback]] recon**, try finding *reset tokens for admin accounts* as to escalate the account takeover

6. If there are **multiple steps** for one function, **try skipping** some of them and check if the functionality still works 

7. Refer-based access control may just check **referer header** for access to sensitive functionality, try including sensitive endpoint like **/admin** in the **referer header** to access sensitive functionalities of admin

9. **IDOR** can occur when there's direct reference to database objects or static files like http://someurl?userid=1 or http://someurl?file=1.txt
10. Always try **[[HTTP parameter pollutio]]** to achieve an IDOR
11. Lot of times parameters are present where there's no need for them, for example **change password** request should not include the **email** param as the application can use the **session token** to validate what user's requesting password reset. In such cases try and manipuate these parameters
12. Try brute-forcing last to characters of UUID with a non-harmful request
13. **Fuzz known API paths** for more endpoints and find **API docs** of the particular org.
14. 
