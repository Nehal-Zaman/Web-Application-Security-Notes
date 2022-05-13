# File upload vulns

# What it is ?

File upload vulnerabilities arise when an application allows a user to upload file without sufficiently checking the name, type, size or contents of the file. This allows the user to upload any arbitrary potentially malicious file.

# Impact of a file upload functionality

The impact depends on 2 factors - 

- The property of the file which the server could not properly validate - size, type, content or name.
- The restrictions imposed on the file after uploaded to the application filesystem.

In the worst case, if the application allows dangerous file types like `.php` or `.jsp`, the attacker can potentially achieve RCE in the application.

If the filename is not validated properly, the well-known files in that OS can be overwritten.

If the size of the file is not validated properly, it can potentially give rise to a DOS.

**NOTE**: The `Content-Type` response header may provide clues as to what kind of file the server thinks it has served.

# File upload with no restriction

From a security perspective, the worst possible scenario is when a website allows you to upload server-side scripts, such as PHP, Java, or Python files, and is also configured to execute them as code. This makes it trivial to create your own web shell on the server.

A simple PHP web-shell will look something like - 

```php
<?php echo system($_GET["cmd"]); ?>
```

This web-shell expects a GET parameter `cmd` that contains the system command, runs it and returns to the user.

# Flawed file type validation

In multipart form-data, there is a `Content-Deposition` header that provides the basic information about the form input it relates to. Apart from this, there is also a `Content-Type` header that gives information about MIME type of the data that is provided as input. If the server explicitly trusts this header and there is no further server-side check, then an attacker can manipulate the `Content-Type` header to bypass file upload check (if it is checking through `Content-Type`) with MIME types like `image/jpeg` or `image/png` which looks legitimate.

# Preventing file execution in user-accessible directories

Apart from preventing users to upload malicious files in the first place, the second line of defense that applications typically implement is to restrict file execution in the server side. However, an attacker can bypass this restriction if it is implemented strictly in the directory where user-uploaded files are stored and some other directory is set loose. The attacker can manipulate the `filename` parameter of multipart-form data to make the server store that uploaded file in a directory where the restriction is set loose.

# Overriding server configuration

Servers use files like `.htaccess` or `Web.config` to configure setting of a specific directory. Normally users can not access these files over HTTP. But sometimes web applications fail to stop an attacker to upload the configuration file of a specific directory. In that case, we can trick the server to map an arbitrary custom extension to a executable MIME type, even the extension is blacklisted. 

# Classic obfuscation techniques

- Make the filename case-insensitive: `exploit.php` → `exploit.pHp`.
- Try providing multiple file extensions → `exploit.php.jpg`.
- Add trailing characters like - white-spaces, dots etc → `exploit.php.`, `exploit.php`
- Try URL encoding (or double URL encoding) dots, forward slashes, backslashes etc.
- Add semicolons or URL encoded null byte characters before the extension → `exploit.php;.jpg`, `exploit.php%00.jpg`.
- Try using multi-byte unicode characters, which may be converted to null bytes and dots after unicode conversion or normalization. Sequences like `xC0 x2E`, `xC4 xAE` or `xC0 xAE` may be translated to `x2E` if the filename parsed as a UTF-8 string, but then converted to ASCII characters before being used in a path.
- Other defenses involve stripping or replacing dangerous extensions to prevent the file from being executed. If this transformation isn't applied recursively, you can position the prohibited string in such a way that removing it still leaves behind a valid file extension. → `exploit.ph.phpp` → `exploit.php` (`.php` is removed)

# Flawed validation of file contents

A more secure server will always check the file content without trusting the `Content-type` header blindly. But there can be flaws in how the server is validating the contents of the file. For example if the server is relying on magic bytes to validate the contents of a file, an attacker can create a malicious file using the required magic bytes and upload it to the server.

# Exploiting file upload race conditions

Most of the time, developers make their own file upload validation process. Before uploading the file, they first save the file into a temporary location, then do some validations after which they decide if the file should be uploaded or not. If the temporary location is accessible to the attacker, he can use the fraction of time before the validation finishes to execute the file from temporary location. However, most of the time, the attacker needs to have access to the source code.

This is how the race conditions in file uploading mechanisms are exploited.

This sort of race conditions can also be exploited if the attacker can upload file by giving a URL. The server first needs to request and save the file, then validate it. If there is sufficient time, you can execute files here as well. Try to make the file larger by adding heading bytes and trailing bytes that will buy some time for the attack.

# Uploading malicious client side scripts

Apart from RCE, you can also upload files like HTML or SVG (if they are applicable) which can contain tags like `<script>` to run client side scripts.

# Exploiting vulnerabilities in the parsing of uploaded files

If the uploaded file seems to be both stored and served securely, the last resort is to try exploiting vulnerabilities specific to the parsing or processing of different file formats. For example, you know that the server parses XML-based files, such as Microsoft Office `.doc` or `.xls` files, this may be a potential vector for XXE injection attacks.

# Uploading files using the PUT method

It's worth noting that some web servers may be configured to support `PUT` requests. If appropriate defenses aren't in place, this can provide an alternative means of uploading malicious files, even when an upload function isn't available via the web interface.

```php
PUT /images/exploit.php HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-httpd-php
Content-Length: 49

<?php echo system($_GET["cmd"]); ?>
```

You can try sending `OPTIONS` requests to different endpoints to test for any that advertise support for the `PUT` method.

# REFERENCES:

[PayloadsAllTheThings/Upload Insecure Files at master · swisskyrepo/PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files)