---
layout: default
title: "Usage"
permalink: /usage/
author_profile: true
---

<a id="usage"></a>
# [Part 1: Everyday Mininet-WiFi Usage](#part1)


<a id="interact"></a>
## [Interact with Stations and APs](#interact)

Start a minimal topology and enter the CLI:
```
$ sudo mn --wifi
```

The default topology is the minimal topology, which includes one OpenFlow kernel AP connected to two stations, plus the OpenFlow reference controller. This topology could also be specified on the command line with --topo=minimal. Other topologies are also available out of the box; see the --topo section in the output of mn -h.

All four entities (2 stations processes, 1 ap process, 1 basic controller) are now running.

If no specific test is passed as a parameter, the Mininet-WiFi CLI comes up.

Display Mininet CLI commands:
```
mininet-wifi> help
```

Display Nodes:
```
mininet-wifi> nodes
```

If the first string typed into the Mininet-WiFi CLI is a station, ap or controller name, the command is executed on that node. Run a command on a station process:
```
mininet-wifi> sta1 ifconfig -a
```

You should see the station’s sta-wlan0 and loopback (lo) interfaces. Note that this interface (sta1-wlan0) is not seen by the primary Linux system when ifconfig is run, because it is specific to the network namespace of the host process.

In contrast, the ap by default runs in the root network namespace, so running a command on the ``ap`` is the same as running it from a regular terminal:
```
mininet-wifi> ap1 ifconfig -a
```

Getting information from node.params
```
py sta1.params
```
Getting information of the wireless network interfaces
```
py sta1.wintfs
```
Optionally, you can get some other information of the interface
```
py sta1.wintfs[0].txpower
```
The same can be done for rssi, mode, channel, freq, range, ip, ip6, etc.


<a id="supportedModes"></a>
### [Supported Wireless Modes](#supportedModes)

Mininet-WiFi supports IEEE 802.11a,b,g,b,p,ax,ac, etc. You can basically use all the modes supported by `hostapd` and `wpa_supplicant`. For example:
```
$ sudo mn --wifi --mode=g --channel=6
$ sudo mn --wifi --mode=a --channel=36
$ sudo mn --wifi --mode=n --freq=5 --channel=36
$ sudo mn --wifi --mode=n5 --channel=6  # for 5GHz
$ sudo mn --wifi --mode=ax --channel=36
```

<a id="connectivity"></a>
## [Test connectivity between stations](#connectivity)

Now, verify that you can ping from station1 to station2:
```
mininet-wifi> sta1 ping -c1 sta2
```

You should see a much lower ping time for the second try (< 100us). A flow entry covering ICMP ping traffic was previously installed in the switch, so no control traffic was generated, and the packets immediately pass through the switch.

An easier way to run this test is to use the Mininet-WiFi CLI built-in pingall command, which does an all-pairs ping:
```
mininet-wifi> pingall
```

Exit the CLI:

```
mininet> exit
```

If Mininet crashes for some reason, clean it up:

```
$ sudo mn -c
```

<a id="wiredlink"></a>
## [Creating wired link between sta and ap](#wiredlink)

You can create a wired link between station and access point with cls=TCLink, as shown below:

```
from mininet.link import TCLink
..
..

net.addLink(sta1, ap1, cls=TCLink)
```