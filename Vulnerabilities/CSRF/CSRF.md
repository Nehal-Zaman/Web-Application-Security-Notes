# What is CSRF ?

- It is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform.

# Conditions for CSRF - 

- **A relevant action** - There is an action within the application that the attacker has a reason to induce.
- **Cookie based session handling** - The application relies solely on `session cookies` to identify the user who made the request. There is no other mechanism in place for tracking sessions or validating user requests.
- **No unpredictable request parameters** - The requests that perform the action do not contain any parameters whose values the attacker can not determine or guess.

# An example - 

- Consider the below HTTP request - 

```
POST /email/change HTTP/1.1  
Host: vulnerable-website.com  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 30  
Cookie: session=yvthwsztyeQkAPzeQ5gHgTvlyxHfsAfE  
  
email=wiener@normal-user.com
```

- Here all three conditions for `CSRF` attack are met - 
	- A relevant action of `changing email` functionality is of interest. With this, an attacker can later on issue a `password reset` and ultimately full user compromise.
	- The application is validating user request solely based on `session cookies`.
	- All the parameters in the request are known to the attacker.

- The attacker can create a web page containing the below `HTML` - 

```html
<html>  
  <body>  
    <form action="https://vulnerable-website.com/email/change" method="POST">  
      <input type="hidden" name="email" value="pwned@evil-user.net" />  
    </form>  
    <script>  
      document.forms[0].submit();  
    </script>  
  </body>  
</html>
```

- When the victim user visits the web page, the below things happen - 
	- The attacker page will trigger an `HTTP` request to the vulnerable website.
	- If the user is logged in to the vulnerable website, their browser will automatically include their `session cookie` in the request (assuming SameSite cookie is not being used).
	- The vulnerable website will process the request in the normal way, treating it as having been made by the victim user, and change their email address.

- Some simple CSRF exploits employ the GET method and can be fully self-contained with a single URL on the vulnerable web site.
- For example - `<img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net">`.

# Preventing CSRF attacks - 

- Include a `CSRF` token within relevant requests.
- The token must be - 
	- Unpredictable with high entropy.
	- Tied to the user's session.
	- Strictly validated in every case before relevant action is executed.

- More on `CSRF tokens` [here](https://portswigger.net/web-security/csrf/tokens).
- An additional defense that is partially effective against `CSRF`, and can be used in conjunction with `CSRF tokens`, is `SameSite cookies`.
- More on SameSite cookies [here](https://portswigger.net/web-security/csrf/samesite-cookies).

# Common CSRF vulnerabilities - 

- Most common `CSRF` vulnerabilities arise due to mistakes made in the validation of `CSRF` tokens.
- Consider the below request.

```
POST /email/change HTTP/1.1  
Host: vulnerable-website.com  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 68  
Cookie: session=2yQIDcpia41WrATfjPqvm9tOkDvkMvLm  
  
csrf=WfF1szMUHhiokx9AHFply5L2xAOfjRkE&email=wiener@normal-user.com
```

- The application no longer relies solely on cookies for session handling.
- The request contains a parameter whose value an attacker can not determine.

#### Validation of CSRF token depends on the request method -
- Some applications correctly validate the token when the request uses the `POST` method but skip the validation when `GET` method is used.
- Attacker can switch to `GET` method to bypass the protection & deliver a `CSRF` attack.

#### Validation of CSRF token depends on token being present - 
- Some applications correctly validate the token when it is present, but skip it if the token is emitted.
- In such case, the attacker can remove the `CSRF` token parameter and launch an attack.

#### CSRF token not tied to the user session -
- Some applications do not validate if the token belongs to the same person who is making the request.
- The application maintains a global pool of tokens that is issued and accepts any token that appears in this pool.
	- In such cases, the attacker can log into his own account, obtain a valid token, and then use that token for the victim user in `CSRF` attack.

#### CSRF is tied to a non-session cookie - 
- Some applications do tie `CSRF` token to a cookie, but not to the same cookie that is used to track sessions.
- The attacker can log in to their own account, obtain a valid token and associated cookie, leverage any **cookie-setting** behaviour to place their cookie into the victim's browser, and feed their token to the victim in their `CSRF` attack.

```
POST /email/change HTTP/1.1  
Host: vulnerable-website.com  
Content-Type: application/x-www-form-urlencoded  
Content-Length: 68  
Cookie: session=pSJYSScWKpmC60LpFOAHKixuFuM4uXWF; csrfKey=rZHCnSzEp8dbI6atzagGoSYyqJqTz5dv  
  
csrf=RhV7yQDO0xcq9gLEah2WVbmuFqyOq7tY&email=wiener@normal-user.com`
```

#### CSRF token simply duplicated in a cookie - 

- Some applications do not maintain any server-side record of tokens that have been issued.
- They instead duplicate each `token` within a cookie and a request parameter.
- When the subsequent request is validated, the application simply verifies that the `token` submitted in the request parameter matches the value submitted in the cookie. This is called `Double submit`.
- In this situation, the attacker can again perform a `CSRF attack` if the web site contains any cookie setting functionality.
- Leverage the cookie-setting behavior to place their cookie into the victim's browser, and feed their `token` to the victim in their `CSRF attack`.


# Referer-based defenses against CSRF - 

- Some applications make use of the HTTP `Referer` header to attempt to defend against CSRF attacks.
- The `Referer` header verifies that the request originated from the application's own domain.

#### Validation of Referer depends on header being present - 

- Some applications validate the `Referer` header when it is present in requests but skip the validation if the header is omitted.
- In this situation, an attacker can craft their `CSRF` exploit in a way that causes the victim user's browser to drop the `Referer` header in the resulting request.
- The easiest way to do this is using a META tag within the HTML page that hosts the `CSRF attack`: `<meta name="referrer" content="never">`.

#### Validation of Referer header can be circumvented - 

- Some applications validate the `Referer` header in a naive way that can be bypassed.
- If the application validates that the domain in the `Referer` starts with the expected value, then the attacker can place this as a subdomain of their own domain ->  `http://vulnerable-website.com.attacker-website.com/csrf-attack`.
- If the application simply validates that the `Referer` contains its own domain name, then the attacker can place the required value elsewhere in the URL -> `http://attacker-website.com/csrf-attack?vulnerable-website.com`.
- In an attempt to reduce the risk of sensitive data being leaked, many browsers now strip the query string from the `Referer` header by default.
	- You can override this behavior by making sure that the response containing your exploit has the `Referrer-Policy: unsafe-url` header set.

# Useful links - 

- [https://allabouthack.com/what-is-csrf-attack-common-csrf-bypass/](https://allabouthack.com/what-is-csrf-attack-common-csrf-bypass/)
- [https://0xffsec.com/handbook/web-applications/csrf/](https://0xffsec.com/handbook/web-applications/csrf/)
- [https://vickieli.dev/csrf/bypass-csrf-protection/](https://vickieli.dev/csrf/bypass-csrf-protection/)

