---
layout: default
title: "FAQ"
permalink: /faq/
author_profile: true
---

# FAQ

### I do not have mac80211_hwsim. How can I install it?

Start a minimal topology and enter the CLI:
```
sudo apt-get install linux-image-extra-`uname -r`
```

### How can I control my Mininet-WiFi nodes remotely?
It's trivial to control Mininet-WiFi nodes from the CLI or from within a Python script running locally, but what if you want some other process or even another computer on your LAN to be able to control your Mininet network remotely?  

Well, there are lots of ways to do this. One idea is that anything you can do in Python, you can do in Mininet-WiFi, and it's often very easy to do so. For example, there are all sorts of frameworks available for any kind of messaging you can imagine.   

Another easy way to control Mininet-WiFi nodes is to use the util/m script. For example:
```
~/mininet-wifi$ util/m sta1 ifconfig
```
Another way is by using sockets. See [https://mininet-wifi.github.io/part4](https://mininet-wifi.github.io/part4)

### The mode N supports booth 2.4 and 5Ghz. How can I make a choice between 2.4 or 5Ghz? You have to set band=2.4 or band=5 when you add an AP. For example:
```
net.addAccessPoint(... band='2.4')
```

### Is it possible to create a wired link between station and ap?
Yes. When you add a link between station and ap you have to add the parameter link=’wired’, for example:
```
net.addLink(sta1, ap2, link='wired')
```

### How to uninstall Mininet-WiFi?
``` 
sudo rm -rf /usr/local/bin/mn /usr/local/bin/mnexec /usr/local/lib/python*/*/*mininet* /usr/local/bin/ovs-
* /usr/local/sbin/ovs-*
``` 