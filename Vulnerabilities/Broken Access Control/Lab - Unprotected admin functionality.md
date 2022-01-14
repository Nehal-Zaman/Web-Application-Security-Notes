# Lab: Unprotected admin functionality

- The `robots.txt` file discloses the admin functionality page at `/administrator-panel`.
- The server is not checking if the user who is accessing the page is authenticated and authorized.
- This is a case of vertical privilege escalation through unprotected admin functionality.