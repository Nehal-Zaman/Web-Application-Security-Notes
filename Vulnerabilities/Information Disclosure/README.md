### What is information disclosure ?

**Information disclosure** is when a website unintentionally reveals sensitive information to its users. Depending on the context, the website may leak all kinds of information to a potential attacker - 

1. Data about other users, such as usernames or financial information.
2. Sensitive commercial or business data.
3. Technical details about the website and its infrastructure.

### Examples of information disclosure

Some basic examples of information disclosure are as follows:

1.  Revealing the names of hidden directories, their structure, and their contents via a `robots.txt` file or directory listing
2.   Providing access to source code files via temporary backups
3.  Explicitly mentioning database table or column names in error messages
4.  Unnecessarily exposing highly sensitive information, such as credit card details
5.  Hard-coding API keys, IP addresses, database credentials, and so on in the source code
6.  Hinting at the existence or absence of resources, usernames, and so on via subtle differences in application behavior

### How do information disclosure vulnerabilities arise ? 

1. Failure to remove internal content from public content.
2. Insecure configuration of the website and related technologies.
3. Flawed design and behavior of the application.

### Exploiting information disclosure vulnerabilities - 

1. `Fuzzing`
2. `Burp Scanner`
3. `Using burp engagement tools` - 
	1. `Search`
	2. `Find comments`
	3. `Discover content`
4. `Engineering informative resources`

### Common sources of information disclosure - 

1. `Files for web crawlers` - Many websites provide files at `/robots.txt` and `/sitemap.xml` to help crawlers navigate their site.
2. `Directory listing` - Web servers can be configured to automatically list the contents of directories that do not have an index page present. This can aid an attacker by enabling them to quickly identify the resources at a given path, and proceed directly to analyzing and attacking those resources.
3. `Developer comments` - During development, in-line HTML comments are sometimes added to the markup. These comments are typically stripped before changes are deployed to the production environment. However, comments can sometimes be forgotten, missed, or even left in deliberately because someone wasn't fully aware of the security implications.
4. `Error messages` - One of the most common causes of information disclosure is verbose error messages. As a general rule, you should pay close attention to all error messages you encounter during auditing.
5. `Debugging data` - Debug messages can sometimes contain vital information for developing an attack, including:
	-   Values for key session variables that can be manipulated via user input
	-   Hostnames and credentials for back-end components
	-   File and directory names on the server
	-   Keys used to encrypt data transmitted via the client
	- Debugging information may sometimes be logged in a separate file. If an attacker is able to gain access to this file, it can serve as a useful reference for understanding the application's runtime state. It can also provide several clues as to how they can supply crafted input to manipulate the application state and control the information received.
6. `User account pages` - Most websites will take steps to prevent an attacker from simply changing parameter to access arbitrary users' account pages. However, sometimes the logic for loading individual items of data is not as robust. An attacker may not be able to load another users' account page entirely, but the logic for fetching and rendering the user's registered email address, for example, might not check that the `user` parameter matches the user that is currently logged in.
7. `Source code disclosure via backup files` - Requesting a code file using a backup file extension can sometimes allow you to read the contents of the file in the response.
8. `Information disclosure due to insecure configuration` - Websites are sometimes vulnerable as a result of improper configuration. This is especially common due to the widespread use of third-party technologies, whose vast array of configuration options are not necessarily well-understood by those implementing them.
9. `Version control history` - Virtually all websites are developed using some form of version control system, such as Git. By default, a Git project stores all of its version control data in a folder called `.git`. Occasionally, websites expose this directory in the production environment. In this case, you might be able to access it by simply browsing to `/.git`.

