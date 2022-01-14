# Lab: User ID controlled by request parameter

- After logging in with `wiener:peter`, we can see an endpoint `/my-account?id=wiener`.
- If we modify the value to `/my-account?id=carlos`, we can access Carlos's API key.
- It is a simple case of horizontal BAC through guessable user ID.