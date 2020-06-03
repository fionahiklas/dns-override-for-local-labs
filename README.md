# DNS override for local labs

## Overview

Classically you would hack the `hosts` file on a developer machine so that name resolution for 
VMs, local servers, or labs can be setup without going to the extreme of creating a domain and 
running BIND or similar.

DNS resolution and selection of name servers is a little more complicated now so some more work
is needed to get a fully-functioning setup work.


## Setup

### Using dnsmasq

TODO: Add details about how to configure and run dnsmasq standalone or in a container



### Add DNS to system

Once you have dnsmasq up and running you need to add it as a nameserver.


#### Linux




#### MacOSX

List the available network services

```
networksetup -listallnetworkservices
```

Produces a list something like this

```
LPSS Serial Adapter (1)
LPSS Serial Adapter (2)
Single RS232-HS
USB-C Lan
Wi-Fi
Bluetooth PAN
Thunderbolt Bridge
```

List DNS servers for a service

```
networksetup -getdnsservers Wi-Fi
```


List search domains for a service

```
networksetup -getsearchdomains Wi-Fi
```

TODO: Actually looks like [scutil](https://ss64.com/osx/scutil.html) may be more pertinent here 



## References

* [resolvectl](https://www.freedesktop.org/software/systemd/man/resolvectl.html)
* [systemd-resolved](https://wiki.archlinux.org/index.php/Systemd-resolved)
* [MacOSX scutil](https://ss64.com/osx/scutil.html)
