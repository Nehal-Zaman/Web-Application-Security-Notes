# Recon notes

# Scope domains

- Identifying the seed/root domains of the target.
- For example, `tesla.com`, `tesla.cn`, `[teslamotors.com](http://teslamotors.com)` etc are all root domains for `*.tesla.com`, `*.tesla.cn` and `*.teslamotors.com`.
- The below methods can be used to discover root domains.

## Acquisitions

- Looking for acquisitions that the target has, increases the scope of the target.
- You can search for acquisitions from [crunchbase](https://www.crunchbase.com/), or simply search in google.

## ASN enumeration

- ASN is a unique number that is assigned to every large company.
- If we get ASN number, we can find the IP subnets and root domains for a target.
- Manually, you can find ASN number from: [bgp.he.net](https://bgp.he.net/)
- You can do the same using automation tools: [metabigor](https://github.com/j3ssie/Metabigor) and [ASNlookup](https://github.com/yassineaboukir/Asnlookup).
- You can use Amass to find root domains out of the ASN number for a target company.
    - `amass intel -asn 46489`

## Reverse WHOIS

- Reverse WHOIS is used to find out the names of organizations who previously owned the target and emails if any.
- Website to look for reverse WHOIS: [WHOXY](https://www.whoxy.com/).
- Automation tool that can be used: [DOMlink](https://github.com/vysecurity/DomLink).

## Ad/Analytics relationships

- You can find related subdomains/domains by looking at the target’s ad/analytics tracker code.
- You can use the site BuiltWith to do the same.

## Google-Fu

- You can use the target’s copyright text, terms of service text and privacy policy text to find related host using google.

## Shodan

- [Shodan](https://www.shodan.io/dashboard) is a tool that continuously spiders infrastructures on the internet.
- It returns response data, cert data, stack profiling data, and more.

# Finding subdomains

## Linked discovery

- A way to widen scope is to examine all the links of our main target.
- We visit a seed/root and recursively spider all the links for a term with regex, examining those links and their links and so on until we have found all the sites that could be in our scope.
- This technique finds both seeds/roots and subdomains as well.
- We can do this using burp pro.
- Automation tools: [GoSpider](https://github.com/jaeles-project/gospider) and [Hakrawler](https://github.com/hakluke/hakrawler).

## Subdomain enumeration with subdomainizer

- [Subdomainizer](https://github.com/nsonaniya2010/SubDomainizer) finds subdomains referenced in javascript files.
- It also finds cloud services referenced in javascript files.
- It can also figure out potentially sensitive items in javascript files.
- However, if only looking for subdomains, you should try [subscraper](https://github.com/Cillian-Collins/subscraper) because it has recursions.

## Subdomain scraping

- Scraping domain information from all source of projects that expose databases of URLs or domains.
- Google dorking: e.g. `site:twitch.tv -www.twitch.tv`.
- Amass: `amass -d twitch.tv`
- Shodan scraper: [shosubgo](https://github.com/incogbyte/shosubgo)
- Other tools: [subfinder](https://github.com/projectdiscovery/subfinder), [github-subdomains.py](https://github.com/gwen001/github-search)
- Subdomain scraping using cloud ranges (TO LEARN)

## Subdomain brute force

- Tools: [Massdns](https://www.notion.so/Recon-notes-38e71b4a1b9747899d2381bc2fc1aca0), [amass](https://www.notion.so/Recon-notes-38e71b4a1b9747899d2381bc2fc1aca0), [shuffleDNS](https://github.com/projectdiscovery/shuffledns)
- Wordlist: all.txt from jhadix, [commonspeak2](https://github.com/assetnote/commonspeak2) from assetnote.

## Alteration scanning

- You may find a naming pattern in the subdomains of a target.
- There can also be subdomains which follow the same naming structure which you did not found yet.
- Tool: [altdns](https://github.com/infosec-au/altdns)

# Others

## Port scanning

- Probe for ports other than 80/443 that might be open in the list of domains and subdomains.
- Tool: [masscan](https://github.com/robertdavidgraham/masscan)
- Use [dnmasscan](https://github.com/rastating/dnmasscan) to resolve domains to IPs so that masscan can use it.

## Service scanning and brutespray

- Use NMAP to figure out the services running on the IPs & ports found by masscan.
- You can scan for default password using a tool called [BruteSpray](https://github.com/x90skysn3k/brutespray).

## Github dorking

- Developers often push sensitive data to public repos thinking that they had set the repo private
- Dorks: [\@here](https://github.com/random-robbie/keywords/blob/master/keywords.txt)
- Automation tool: [github-search](https://github.com/gwen001/github-search) by gwen001

## Screenshoting

- [Aquatone](https://github.com/michenriksen/aquatone)
- [HTTPscreenshot](https://github.com/breenmachine/httpscreenshot)
- [Eyewitness](https://github.com/FortyNorthSecurity/EyeWitness)

## Subdomain takeover

- Signatures for unclaimed subdomains: [can-i-take-over-xyz](https://github.com/EdOverflow/can-i-take-over-xyz)
- Tools: [SubOver](https://github.com/Ice3man543/SubOver) and [nuclei](https://github.com/projectdiscovery/nuclei)