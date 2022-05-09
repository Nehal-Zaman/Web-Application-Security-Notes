### RECON

- Project Tracking - `Xmind`, `Obsidian` - anything to track the project.
- Wide Recon - Art of discovering as many assets related to `target` as possible.
	- Scope domains
	- Acquisitions
	- ASN Enumeration
	- Reverse WHOIS
	- Subdomain Enumeration
	- Port Analysis
	- Other - 
		- Vulnerabilities - `Subdomain takeovers`, `Buckets`, `Github leaks`.
		- Automation - Interlace, Screenshoting, Frameworks

- Start with root domains.
	- Understand the company.

- Identify the company acquisitions.
	- Company's acquisitions can be investigated from [Crunchbase](https://crunchbase.com)

- ASN Enumeration.
	- ASNs can be identified from [here](http://bgp.he.net)
	- Command line tool - `metabigor`, `asnlookup`
	- ASN enumeration with `Amass`

- Reverse WHOIS.
	- Find pieces of information from [WHOXY](https://whoxy.com)
	- Command line tool - `DOMlink`

- Ad/Analytics Relationships.
	- Glean related domains & sub-domains by looking at a target's ad/analytics tracker codes.
	- [Builtwith](https://builtwith.com) can be used for this.

- Google-Fu
	- Use google dorks to glean related hosts.

- Shodan
	- It is a tool that continuously spiders infrastructure on the internet.
	- It can be found [here](https://shodan.io)

- Find subdomains
	- Linked & JS discovery.
		- `Burpsuite pro` can be used for spidering.
		- Command line tools - `GoSpider`, `Hakrawler`.
		- Subdomain enumeration with `Subdomainizer`, `subscraper`.
	- Subdomain scraping.
		- Google dorks, e.g - `site:example.com -www.example.com` (minus out `www.example.com`)
		- `Amass`, `Subfinder`, `github-subdomains.py`
		- `shosubgo` - gather subdomains from `shodan.io`
		- Enumerate the cloud ranges to find subdomains - [here](https://tls.bufferover.run/dns?q=.twitch.tv)
	- Subdomain bruteforce.
		- `Massdns`, `Amass`
		- `Shuffledns`
		- Massive subdomain wordlist - `all.txt` from Jason Haddix
		- Another wordlist - `commonspeak2-wordlists` from github

	-	Alteration Scanning - 
		-	When bruteforcing/gathering subdomains via scraping, you may come across a naming patterns in this subdomains.
		-	`dev.company.com`, `dev1.company.com`, `dev-1.company.com`
		-	Even though you may not have found it yet, there may be other targets that conform to naming conventions.
		-	First tool - `altdns`
		-	`Amass` can be used.


### Other

- Port Analysis - 
	- `masscan` instead of `nmap`
	- `dnmasscan` - resolves domains to IP that can used in masscan.
	- Use `nmap` to enumerate services running on ports found in `masscan` results.
	- `brutespray` to test for default credentials for services running on those ports.

- Github dorking
	- Jason Haddix has a list on github for dorks.
	- `github-search`  has some tools for dorking as well.

- Screenshoting - 
	- `Aquatone`, `HTTPscreenshot`, `Eyewitness`
	- `httprobe` to verify if a domain is active.

- Subdomain Takeovers - 
	- `Can-i-takeover-xyz` by `EdOverflow` on github.
	- `subover`, `nuclei`	

- Automation++ - 
	- `interlace` by `Codingo` to glue together a recon framework
	- `Tomnomnom` has an `extensive-list` of tools
