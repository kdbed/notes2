---
title: Subdomain Enumeration
tags:
  - infosec
  - pentest
---




## From DNS

```shell
dig +nocmd trick.htb axfr +noall +answer @trick.htb
```


## [[Wfuzz]]

In case the subdomain has a certain naming convention (Trick/HTB):

```shell
sed 's/^/preprod-/' subdomains-top1million-110000.txt
```

Then use [wfuzz]({{<relref "wfuzz.md#" >}}):

```shell
sudo wfuzz -c -f out -w wordlist -u "http://trick.htb" -H "Host: FUZZ.trick.htb"  --hw 475
```