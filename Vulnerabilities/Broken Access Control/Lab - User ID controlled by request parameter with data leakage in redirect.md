# Lab: User ID controlled by request parameter with data leakage in redirect

- After logging in with `wiener:peter`, we can see a user ID parameter, the value of which can be guessed.
- However, the server has the mechanism to identify correct access controls and redirects us to login page.
- Even though the server is redirecting, we can see the modified user ID's profile if we intercept the `302` response.
- It is a case of horizontal BAC through data leakage in redirect.
