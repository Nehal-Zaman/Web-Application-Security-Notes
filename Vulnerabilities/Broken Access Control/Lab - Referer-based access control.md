# Lab: Referer-based access control

- The application is relying on `Referrer` header for access control.
- User upgrade functionality can only be possible if the `Referrer` has value -> `https://your-url.com/admin`.
- Simply make a `GET` request to `/admin-roles` with parameters `username=wiener` and `action=upgrade`.
- Intercept the request in burp.
- Add the necessary `Referrer` header.
