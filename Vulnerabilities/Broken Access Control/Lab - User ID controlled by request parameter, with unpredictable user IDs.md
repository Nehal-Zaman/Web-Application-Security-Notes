# Lab: User ID controlled by request parameter, with unpredictable user IDs

- The application is using `GUID` to identify a user.
- So, the identifier is hard to guess.
- However, one of the blog post reveals the `GUID` for `carlos`.
- The application allows anyone to modify the identifier since it is used in a request parameter.
- We can simple login as wiener.
- Then replace his `GUID` with that of carlos.
- Now we have access to Carlos's account.
