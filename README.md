# DNS override for local labs

## Overview

Classically you would hack the `hosts` file on a developer machine so that name resolution for 
VMs, local servers, or labs can be setup without going to the extreme of creating a domain and 
running BIND or similar.

DNS resolution and selection of name servers is a little more complicated now so some more work
is needed to get a fully-functioning setup work.


## Setup

### Using dnsmasq

Using [dnsmasq](https://wiki.debian.org/dnsmasq) to set this up.

After much hacking around it seems that `dnsmasq` can't be persuaded to connect to only the VMWare network 
interface or it's address.  It seems to obstinately connect to `0.0.0.0` all the time.  The only solution I've
found is to run it in a docker container, an [excellent image](https://hub.docker.com/r/andyshinn/dnsmasq) is 
available on [Docker hub](https://hub.docker.com/) run using the following command

```
docker run -d -p 127.0.2.53:53:53/tcp -p 127.0.2.53:53:53/udp --cap-add=NET_ADMIN andyshinn/dnsmasq:2.75 \
--address=/some.test.server.interal/10.0.1.2 \
--address=/another.test.server.com/10.3.1.2 \
--address=/wibble.plop.feegle.ribbit/10.90.42.2
```

Test that resolution works

```
dig wibble.plop.feegle.ribbit @127.0.2.53
```

The above works fine on Linux as it seems that the loopback device is able to host any address in the 
127.0.0.0/8 subnet

TODO: Check if loopback behaves in the same way on MacOS and Windows



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
* [Docker internals, namespaces](https://medium.com/@BeNitinAgarwal/understanding-the-docker-internals-7ccb052ce9fe)

