---
title:  "Wireguard VPN Plugin for NetworkManager in GNOME"
categories:
  - sysadmin
tags:
  - vpn
  - wireguard
  - networkmanager
  - gnome
last_modified_at: 2021-01-27T01:58:11+02:00
---

Wireguard is a layer-3 secure network tunnel for ipv4 and ipv6. It has managed to remove the complexity that other solutions/VPN protocols like OpenVPN or IPSec brought, and also providing better performance.

Almost a year after being incorporated as a module in the linux kernel, this vpn solution continues growing and is becoming more and more popular.

So far it's all praise. The only thing that I am still not used to is the stateless connections that Wireguard has by design. It means that we won't have logging, peer connection status, etc.

I was also missing some sort of integration with NetworkManager. That's why I ended up installing a plugin for NetworkManager in GNOME. I can now connect/disconnect the VPN, as I do with OpenVPN.

Thanks to the [network-manager-wireguard](https://github.com/max-moser/network-manager-wireguard) repository it's pretty easy to achieve. This is basically how I did it on Debian Buster:

```
$ sudo apt install build-essential libgtk-3-dev libnma-dev libsecret-1-dev

$ git clone https://github.com/max-moser/network-manager-wireguard
$ cd network-manager-wireguard

$ ./autogen.sh --without-libnm-glib

Output:
Build configuration: 
  --with-gnome=yes
  --with-libnm-glib=no
  --enable-absolute-paths=no
  --enable-more-warnings=yes
  --enable-lto=no
  --enable-ld-gc=yes


$ ./configure --prefix=/usr \
              --without-libnm-glib \
              --sysconfdir=/etc \
              --libdir=/usr/lib/x86_64-linux-gnu \
              --libexecdir=/usr/lib/NetworkManager \
              --localstatedir=/var

$ make   
$ sudo make install
```

Once installed, we can create a wireguard type connection:

![Wireguard Connection](/assets/images/wireguard-networkmanager-type.png)


The configuration itself is very simple, and not all fields are required. The only thing I haven't been able to do is add more than one DNS server, it doesn't seem to like the comma as separator.

![Wireguard Config](/assets/images/wireguard-networkmanager-config.png)


Enjoy!
