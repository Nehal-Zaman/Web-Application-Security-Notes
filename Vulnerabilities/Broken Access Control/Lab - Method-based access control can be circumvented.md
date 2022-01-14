# Lab: Method-based access control can be circumvented

- After logging as admin, we can see that we can make a `POST` request to `/admin-role` with parameters `username` and `action` (value = `upgrade` to escalate to high privileged user).
- However, the same function can be executed if we use `GET` method instead of `POST`.
- We simply login with low-privileged user and make a `GET` request to `/admin-role` with the necessary parameters and grant us admin rights.
