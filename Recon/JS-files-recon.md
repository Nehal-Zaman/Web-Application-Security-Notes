# JS file analysis

# What JS file may contain?

- New endpoints.
- Hidden parameters.
- API keys (be careful with this; API keys can be meant for public as well)
- Business logic of the application.
- Secrets/passwords.
- DOM sinks leading to `XSS`.

# Is the code packed?

- We can use [Beautifier](https://beautifier.io/) to make the code readable.
- Deobfuscated? We can use online deobfuscator like: [Deobfuscate](https://deobfuscate.io/).

# Manual analysis

- Go to the target application in a browser.
- Right click and go to `view-source` option.
- Search for `.js`.
- Open the `.js` file(s) in new tab(s).
- Beautify/deobfuscate code if needed.
- We are specifically looking for:
    - new and hidden endpoints/parameters.
    - API keys.
    - Secrets/passwords.
- We can also look for developer comments.
- Also look for keywords related to `AJAX` requests:
    - `url:`
    - `POST`
    - `api`
    - `GET`
    - `setRequestHeader`
    - `send(`
    - `.headers`
    - `onreadystatechange`
    - `var {xyz} =`
    - `getParameter(`
    - `parameter`
    - `.example.com` (the domain we are testing)
    - `apiKey`
- Other common keywords that we can look for:
    - `postMessage`
    - `messageListener`
    - `innerHTML`
    - `document.write(`
    - `document.cookie`
    - `location.href`
    - `redirectUrl`
    - `window.hash`
- It all depends on the target and your creativity, what keyword you are looking for.

# Automation tools

- `getJS` by [003random](https://github.com/003random/getJS.git): It can be used to extract any JS file from a given domain(s).
- `relative-url-extractor` by [jobertabma](https://github.com/jobertabma/relative-url-extractor) and `GoLinkFinder` by [0xsha](https://github.com/0xsha/GoLinkFinder.git): These can be used to find hidden/useful endpoints from JS files.
- One liners:

```
cat domains.txt | katana | grep js | httpx -mc 200 | tee js.txt

nuclei -l js.txt -t ~/nuclei-templates/exposures/ -o js-bugs.txt
OR
--------------------
file="js.txt"

# Loop through each line in the file
while IFS= read -r link
do
    # Download the JavaScript file using wget
    wget "$link"
done < "$file"
--------------------
grep -r -E "aws_access_key|aws_secret_key|api key|passwd|pwd|heroku|slack|firebase|swagger|aws_secret_key|aws key|password|ftp password|jdbc|db|sql|secret jet|config|admin|pwd|json|gcp|htaccess|.env|ssh key|.git|access key|secret token|oauth_token|oauth_token_secret|smtp" *.js
```

# References

- [https://www.youtube.com/watch?v=0jM8dDVifaI](https://www.youtube.com/watch?v=0jM8dDVifaI)
- [https://realm3ter.medium.com/analyzing-javascript-files-to-find-bugs-820167476ffe](https://realm3ter.medium.com/analyzing-javascript-files-to-find-bugs-820167476ffe)
- [https://medium.com/geekculture/analysing-javascript-files-for-bug-bounty-hunters-71e2727abebe](https://medium.com/geekculture/analysing-javascript-files-for-bug-bounty-hunters-71e2727abebe)
- [https://www.securecoding.com/blog/monitoring-javascript-files-for-bugbounty/](https://www.securecoding.com/blog/monitoring-javascript-files-for-bugbounty/)
- [https://beautifier.io/](https://beautifier.io/)
- [https://deobfuscate.io/](https://deobfuscate.io/)