### What is Access Control ?
-> It is the application of constraints on who (or what) can perform attempted actions or access resources that they have requested. It depends on the following - 
- `Authentication` -> It identifies the user and confirms that they are who they say they are.
- `Session Management` -> It identifies which requests are being sent by that user.
- `Access Control` -> It determines whether that user is authorized to perform that function.

`Access Control Security Models` -> [here](https://portswigger.net/web-security/access-control/security-models)

### Categories of Access Controls - 
- `Vertical Access Controls` -> Different types of users have access to different application functions. It restricts access to sensitive functionality that is not available to other types of users.
- `Horizontal Access Controls` -> Different users have access to subset of resources of the same type. It restricts access to resources to the users who are specifically allowed to access those resources.
- `Context-dependent Access Controls` -> It prevents a user performing actions in the wrong order. It restricts access to functionality and resources based upon the state of the application or the user's interaction with it.

### Broken Access Controls - 
-> BAC vulnerabilities exist when a user can access some resource or perform some action that they are not supposed to access.

##### Vertical Privilege Escalation -
-> If a user can gain access to functionality that they are not permitted to access, it is a `vertical privilege escalation`.
- `Unprotected functionality` - At its most basic, vertical privilege escalation arises where an application does not enforce any protection over sensitive functionality.
	- For example, administrative functions might be linked from an administrator's welcome page but not from a user's welcome page.
	- However, a user might simply be able to access the administrative functions by browsing directly to the relevant admin URL.
	- In some cases, the administrative URL might be disclosed in locations, such as the `robots.txt` file.
	- Even if the URL isn't disclosed anywhere, an attacker may be able to use a wordlist to brute-force the location of the sensitive functionality.
	- In some cases, sensitive functionality is not robustly protected.
	- It is concealed by giving it a less predictable URL.
	- Users might still discover the obfuscated URL in various ways.
- `Parameter-based Access Control Methods` - Some applications determine the user's access rights or role at login, and then store this information in a user-controllable location, such as a hidden field, cookie, or preset query string parameter. This approach is fundamentally insecure because a user can simply modify the value and gain access to functionality to which they are not authorized, such as administrative functions.
- `Broken Access Control resulting from platform misconfiguration` - 
	- Some applications enforce access controls at the platform layer by restricting access to specific URLs and HTTP methods based on the user's role.
		- Some application frameworks support various non-standard HTTP headers that can be used to override the URL in the original request, such as `X-Original-URL` and `X-Rewrite-URL`.
	- Some web sites are tolerant of alternate HTTP request methods when performing an action.
		- If an attacker can use the `GET` (or another) method to perform actions on a restricted URL, then they can circumvent the access control that is implemented at the platform layer.

##### Horizontal Privilege Escalation - 
-> It arises when a user is able to gain access to resources belonging to another user, instead of their own resources of that type.
- `User ID controlled by request parameter` -> Some applications allow modifiable user IDs that gives access to a particular user's functions and roles. An attacker can modify the IDs to access other user's profiles.
- `User ID controlled by request parameter, with unpredictable user IDs` -> In some applications, the exploitable parameter does not have a predictable value.
	- Here, an attacker might be unable to guess or predict the identifier for another user.
	- However, the GUIDs belonging to other users might be disclosed elsewhere in the application where users are referenced.
- `User ID controlled by request parameter with data leakage in redirect` -> In some cases, an application does detect when the user is not permitted to access the resource, and returns a redirect to the login page.
	- However, the response containing the redirect might still include some sensitive data belonging to the targeted user, so the attack is still successful.

##### Horizontal to vertical privilege escalation -
- Sometimes, horizontal privilege escalation leads to the compromise of a privileged account.
- With this privileged account, it might be possible to retrieve/reset the account's password.
- Then the attacker can access privileged function, thereby converting horizontal PE into a vertical PE.

##### Insecure Direct Object Reference (IDOR) - 
- It is a type of access control vulnerability that arises when an application uses user-supplied input to access objects directly.
- IDOR vulnerabilities are most commonly associated with horizontal privilege escalation, but they can also arise in relation to vertical privilege escalation.
- `IDOR vulnerability with direct reference to database objects` - 
	- Example - `https://insecure-website.com/customer_account?customer_number=132355`
	- Here, an attacker can modify the `customer_number` parameter to access other user's account.
- `IDOR vulnerability with direct reference to static files` -
	- Example - `https://insecure-website.com/static/12144.txt`
	- Here, an attacker can modify the incrementing file name to access other static files.

##### Access control vulnerabilities in multi-step processes - 
- Many web sites implement important functions over a series of steps.
- For example, administrative function to update user details might involve the following steps - 
	- Load form containing details for a specific user.
	- Submit changes.
	- Review the changes and confirm.
- Sometimes, a web site will implement rigorous access controls over some of these steps, but ignore others.
- Here, an attacker can gain unauthorized access to the function by skipping the first two steps and directly submitting the request for the third step with the required parameters.

##### Referer-based access control - 
- Some websites base access controls on the `Referer` header submitted in the HTTP request.
- The `Referer` header is generally added to requests by browsers to indicate the page from which a request was initiated.
- For example, suppose an application robustly enforces access control over the main administrative page at `/admin`, but for sub-pages such as `/admin/deleteUser` only inspects the `Referer` header.
- If the `Referer` header contains the main `/admin` URL, then the request is allowed.
- In this situation, since the `Referer` header can be fully controlled by an attacker, they can forge direct requests to sensitive sub-pages, supplying the required `Referer` header, and so gain unauthorized access.

##### Location-based access control - 
- Some web sites enforce access controls over resources based on the user's geographical location.
- These access controls can often be circumvented by the use of web proxies, VPNs, or manipulation of client-side geolocation mechanisms.

