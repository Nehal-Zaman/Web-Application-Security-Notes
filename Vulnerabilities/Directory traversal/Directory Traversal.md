### What is directory traversal ? 

It is a web security vulnerability that allows an attacker to read arbitrary files on the server that is running an application.

In some cases, the attacker might be able to write to arbitrary files on the server, and ultimately take full control of the server.

### Reading arbitrary files via directory traversal - 

Consider the below `HTML` - 

```html
<img src="/loadImg?name=myimage.png" alt="My image" />
```

The `/loadImg` URL takes a `name` parameter and returns the image content. The image is stored on `/var/www/images/`. To return the image, the application appends the `name` value to the path - `/var/www/images/myimage.png` and then reads it.

If there is no defense mechanism for directory traversal, an attacker can request the following URL to read an arbitrary file on the server - 

```html
https://insecure-website.com/loadImg?name=../../../etc/passwd
```

This causes the application to read - `/var/www/images/../../../etc/passwd` -> `/etc/passwd`.

**UNIX based systems** - `../../../etc/passwd`
**Windows systems** - `..\..\..\windows\win.ini`


### Common obstacles to exploiting file traversal vulnerabilities - 

- If an application strips or blocks directory traversal sequences from user supplied filename, you might be able to use an absolute path from the filesystem root, such as `filename=/etc/passwd` to directly reference a file without using any traversal sequences.
- You might be able to use nested traversal sequences, such as `....//` or `....\/`, which will revert to simple traversal sequences when the inner sequence is stripped.
- In some contexts, such as in a URL path or the `filename` parameter of a `multipart/form-data` request, web servers may strip any directory traversal sequences before passing your input to the application. You can sometimes bypass this kind of sanitization by URL encoding, or even double URL encoding, the `../` characters, resulting in `%2e%2e%2f` or `%252e%252e%252f` respectively. Various non-standard encodings, such as `..%c0%af` or `..%ef%bc%8f`, may also do the trick.
- If an application requires that the user-supplied filename must start with the expected base folder, such as `/var/www/images`, then it might be possible to include the required base folder followed by suitable traversal sequences. Example - `filename=/var/www/images/../../../etc/passwd`
- If an application requires that the user-supplied filename must end with an expected file extension, such as `.png`, then it might be possible to use a null byte to effectively terminate the file path before the required extension. Example - `filename=../../../etc/passwd%00.png`


**REFERENCES** - 

[https://portswigger.net/web-security/file-path-traversal](https://portswigger.net/web-security/file-path-traversal)
[https://github.com/N3H4L/PayloadsAllTheThings/tree/master/Directory%20Traversal](https://github.com/N3H4L/PayloadsAllTheThings/tree/master/Directory%20Traversal)
