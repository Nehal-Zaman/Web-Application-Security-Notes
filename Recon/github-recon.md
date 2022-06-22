# Github Recon

# Finding sensitive information leaks using github:

## Manual approach:

This can be simply done by searching for specific keywords in github search area.

Examples:

1. “Company” password
2. “Company” key
3. “Company” secret
4. “Company” pass
5. “Company” credentials
6. “Company” login
7. “Company” token
8. “Company” ftp
9. “Company” config
10. “Company” pwd

1. “Company” security_credentials → LDAP data (Active directory)
2. “Company” connectionstring → database credentials
3. “Company” JDBC → database credentials
4. “Company” ssh2_auth_password → Unauthorized access to servers.
5. “Company” send_keys or send,keys → if other keywords related to passwords fails.

More keywords [\@here](https://github.com/random-robbie/keywords/blob/master/keywords.txt).

1. user:nehal → search by specific users
2. language:bash → search by specific language

## Automated approach:

1. Gitrob → https://github.com/michenriksen/gitrob

## References:

- [https://securitytrails.com/blog/github-dorks](https://securitytrails.com/blog/github-dorks) ****
- [https://michenriksen.com/blog/gitrob-now-in-go/](https://michenriksen.com/blog/gitrob-now-in-go/)
- [https://gist.github.com/EdOverflow/922549f610b258f459b219a32f92d10b](https://gist.github.com/EdOverflow/922549f610b258f459b219a32f92d10b)