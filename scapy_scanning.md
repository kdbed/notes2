+++
title = "Active Scanning with Scapy"
author = ["svejk"]
draft = false
+++

## Scanning Networks with Scapy {#scanning-networks-with-scapy}

Nmap implements several types of scans and can be used to detect the versions of operating systems and services; it can also perform custom vulnerability scanning.  Here we'll implement a couple of simple scans using `scapy` in Python.

-   <span class="underline">SYN scan</span> : A SYN scan sends a TCP SYN packet to a port and looks for a SYN/ACK packet in response.
-   <span class="underline">DNS scan</span> : A DNS scan tests whether a DNS server is running on the target system.

`Scapy` makes it easy to create and send custom packets over the network and to sniff network traffic for responses.


## Port Scan {#port-scan}

```python
from scapy.all import *
import ipaddress

ports = [25,80,53,443,445,3306,8080,8443]

def SynScan(host):
    ans,unans = sr(IP(dst=host) /
                   TCP(sport=33333, dport=ports, flags="S")
                   ,timeout=2,verbose=0)
    print("Open ports at %s:" % host)
    for (s,r,) in ans:
        if s[TCP].dport == r[TCP].sport and r[TCP].flags == "SA":
            print(s[TCP].dport)

def DNSScan(host):
    ans,unans  = sr(IP(dst=host) /
                    UDP(dport=53) /
                    DNS(rd=1,qd=DNSQR(qname="google.com"))
                    ,timeout=2,verbose=0)
    if ans and ans[UDP]:
        print("DNS Server at %s" % host)

host = input("Enter IP Address: ")
try:
    ipaddress.ip_address(host)
except:
    print("Invalid address")
    exit(-1)

SynScan(host)
DNSScan(host)
```


### SYN Scan {#syn-scan}

<a id="code-snippet--synscanner"></a>
```python
def SynScan(host):
    ans,unans = sr(IP(dst=host) /
                   TCP(sport=33333, dport=ports, flags="S")
                   ,timeout=2,verbose=0)
    print("Open ports at %s:" % host)
    for (s,r,) in ans:
        if s[TCP].dport == r[TCP].sport and r[TCP].flags == "SA":
            print(s[TCP].dport)
```


### DNS Scan {#dns-scan}

<a id="code-snippet--dnsscanner"></a>
```python
def DNSScan(host):
    ans,unans  = sr(IP(dst=host) /
                    UDP(dport=53) /
                    DNS(rd=1,qd=DNSQR(qname="google.com"))
                    ,timeout=2,verbose=0)
    if ans and ans[UDP]:
        print("DNS Server at %s" % host)
```


## Source {#source}

**Python for Cybersecurity** - Howard E Poston