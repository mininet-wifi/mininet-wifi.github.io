---
layout: default
title: "FAQ"
permalink: /faq/
author_profile: true
---

# FAQ

## I do not have mac80211_hwsim. How can I install it?

Start a minimal topology and enter the CLI:
```
sudo apt-get install linux-image-extra-`uname -r`
```

## The mode N supports booth 2.4 and 5Ghz. How can I make a choice between 2.4 or 5Ghz? You have to set band=2.4 or band=5 when you add an AP. For example:
```
net.addAccessPoint(... band='2.4')
```

## Is it possible to create a wired link between station and ap?
Yes. When you add a link between station and ap you have to add the parameter link=’wired’, for example:
```
net.addLink(sta1, ap2, link=’wired’)
```

## How to uninstall Mininet-WiFi?
``` 
sudo rm -rf /usr/local/bin/mn /usr/local/bin/mnexec /usr/local/lib/python*/*/*mininet* /usr/local/bin/ovs-
* /usr/local/sbin/ovs-*
``` 