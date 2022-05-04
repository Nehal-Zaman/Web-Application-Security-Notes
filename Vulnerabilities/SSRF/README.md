# SSRF

# What is it ?

Server Side Request Forgery (SSRF) is a web security vulnerability in which an attacker makes the server application to make a request to a different location.

Typically SSRF can be used to make request to internal systems of an organization which they should not be able to access otherwise.

In other cases, SSRF can be used to make the application to request an external domain, thereby leaking some sensitive data like authorization credentials or API keys.

In some cases, SSRF may be used to achieve Remote Code Execution.

# Common SSRF attacks:

- **SSRF attacks against the server itself**: The attacker induces the application to make a HTTP request back to the server that is hosting the application, via its loopback interface.
- **SSRF attacks against other backend systems**: The attacker induces the application to make a HTTP request to another system in the internal network of the organization which should not be accessible from outside.

# Bypassing common SSRF defenses:

- **SSRF with blacklist based input filters**:
    - Use alternate representation of `127.0.0.1`, such as `2130706433`, `017700000001` or `127.1`.
    - Register a domain name that resolves to `127.0.0.1`. We can use burp collaborator for that.
    - Obfuscate blacklisted strings using URL encoding or case variation.
- **SSRF with whitelist based input filters**:
    - You can embed credentials in a URL before the hostname using the `@` character → `http://expected.com@evil.com`.
    - You can use the `#` character to specify a URL fragment → `https://evil.com/#expected.com`.
    - You can create a subdomain based on the whitelisted string for a domain that you control → `https://expected.com.evil.com`.
    - You can URL-encode characters to confuse the URL-parsing code.
    - You can also combine the above techniques.
- **Bypassing SSRF filters using open redirection**: If there is a whitelist based filter in place that is configured correctly, exploiting SSRF can be pretty tough. However, if there is an open redirect vulnerability in the whitelisted domain itself, SSRF is possible.

# Blind SSRF vulnerabilities

Blind SSRF vulnerabilities arise when an application can be induced to issue a back-end HTTP request to a supplied URL, but the response from the back-end request is not returned in the application's front-end response.

The impact of blind SSRF is comparatively low than normal SSRF due to internal response not being seen in the front end response. However, sometimes blind SSRF may lead to full Remote Code Execution.

The most effective way to exploit a blind SSRF is by using OAST techniques using tools like burp collaborator.

Blind SSRF can used to find other vulnerabilities on the server itself or on other backend systems.You can blindly sweep the internal IP address space, sending payloads designed to detect well-known vulnerabilities.

Another avenue for exploiting blind SSRF vulnerabilities is to induce the application to connect to a system under the attacker's control, and return malicious responses to the HTTP client that makes the connection. If you can exploit a serious client-side vulnerability in the server's HTTP implementation, you might be able to achieve remote code execution within the application infrastructure.

# Finding hidden attack surface for SSRF vulnerabilities

- **Partial URL in requests**: Sometimes, an application places only a hostname or part of a URL path into request parameters. The value submitted is then incorporated server-side into a full URL that is requested.
- **URL in data formats**: Some applications transmit data in formats whose specification allows the inclusion of URLs that might get requested by the data parser for the format. An obvious example of this is the XML data format, which has been widely used in web applications to transmit structured data from the client to the server. When an application accepts data in XML format and parses it, it might be vulnerable to XXE injection, and in turn be vulnerable to SSRF via XXE.
- **SSRF via the Referer header**: The `Referer` header often represents fruitful attack surface for SSRF vulnerabilities.