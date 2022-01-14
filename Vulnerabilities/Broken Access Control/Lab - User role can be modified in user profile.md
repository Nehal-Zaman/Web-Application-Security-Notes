# Lab - User role can be modified in user profile

- After logging with valid creds, an email-changing functionality can be seen.
- We are sending a JSON object having `email` parameter with the value of the new email.
- If we append another parameter `roleid` having value `2` alongside the new email entry, we are essentially using our new email as a privileged user's email.
- Hence, it is a case of vertical BAC.
