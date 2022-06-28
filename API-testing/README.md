# API Pentesting

# What is an API ?

- Stands for Application Programming Interface
- Provides a computer friendly method of interacting with a data source or backend logic
- Used for both mobile and web
- Commonly, API response is in JSON or XML.
- Two types of API → RESTful and GraphQL
- APIs return data directly from the data or some backend logic

# RESTful API

- Have a defined structure which relates to CRUD functionality.

```bash
Create -> POST /posts/
Read -> GET /posts/1
Update -> POST /posts/1, PUT /posts/1, POST /posts/1/edit
Delete -> DELETE /posts/1, POST /posts/1/delete
```

- Can easily predict new endpoints simply by knowing an application.
- They usually return JSON.

# Recon stage

- Fuzzing with common resource names.
- Curate some likely endpoints based on your understanding of the application
- Take a look at API versioning. If `/api/v3/users/1` exists, try for `/api/v1/users/1`.
- Java based web apps have this file `application.wadl`.
    - This file is used by devs as reference. It contains all the endpoints.
- Scan javascript files.
- Look for developer docs
- Check the wayback machine
- Use different HTTP methods (GET, POST, DELETE, PUT) while fuzzing.

# Hacking APIs

- Broken Object Level Authorization (IDOR) → you can access something which you should not be accessing.
    - Method 1
        - Find endpoints with IDs in the request
        - Change ID to another account.
        - If it works it is an IDOR
    - Method 2
        - Find the endpoints that requires admin permissions
        - Login to an account that has guest permissions
        - Repeat the requests to admin endpoints, changing the cookies
        - If it works it is an IDOR
- Broken User Authentication → Its about tokens used for authentication.
    - Unprotected API that are considered internal
    - Weak API keys that are not rotated
    - Default/Weak passwords
    - Credentials/Keys that are included in URLs.
    - Lack of access token validation (including JWT validation)
    - Unsigned/Weakly signed non-expiring JWTs
- Excessive Data Exposure → Information disclosure
    - Call the API
    - Look at the response
    - Is it disclosing too much info ?
    - Enumerate through the API for hidden endpoints
    - Enumerate through the parameters to find hidden parameters.
    - Understand the impact if you find excessive information
- Lack of resources and rate limiting
    - Sending requests at a rate that is exceeding the APIs processing speed
- Broken Functional Level Authorization → privilege escalation
    - There might be administrative functions exposed in the API
    - Non-privileged users can access these funcitons without authorization if they know how.
- Mass Assignment
    - Sometimes API works with the data structures without proper filtering
    - Recieved payload is blindly transformed in an object and stored
- Security Misconfiguration
    - Unpatched systems
    - Unprotected files and directories
    - Unhardened images
    - Missing, outdated, or misconfigured TLS
    - Exposed storage or server management panels
    - Missing CORS policy or security headers
    - Error messages with stack traces
    - Unnecessary features enabled
- Injection
    - Attackers send malicious input to be forwarded to an internal interpreter:
        - SQL
        - NoSQL
        - LDAP
        - OS commands
        - XML parsers
        - Object-Relational Mapping (ORM)
- Improper assets management
- Insufficient logging and monitoring

# REFERENCES

- [https://www.youtube.com/watch?v=qqmyAxfGV9c](https://www.youtube.com/watch?v=qqmyAxfGV9c)
- [https://github.com/omkar-ukirde/api-pentesting](https://github.com/omkar-ukirde/api-pentesting)
- [https://apisecurity.io/encyclopedia/content/owasp/owasp-api-security-top-10.htm](https://apisecurity.io/encyclopedia/content/owasp/owasp-api-security-top-10.htm)