---
layout: default
title: "Part 3"
permalink: /part3/
author_profile: true
---


# Part 3: Mininet-WiFi Command-Line Interface (CLI) Commands

## Display Options
To see the list of Command-Line Interface (CLI) options, start up a minimal topology and leave it running. Build the Mininet-WiFi:

```
$ sudo mn --wifi
```

## Display the options:
```
mininet-wifi> help
```

## Python Interpreter
If the first phrase on the Mininet-WiFi command line is py, then that command is executed with Python. This might be useful for extending Mininet-WiFi, as well as probing its inner workings. Each station, ao, and controller has an associated Node object.

At the Mininet CLI, run:
```
mininet-wifi> py 'hello ' + 'world'
```

Print the accessible local variables:
```
mininet-wifi> py locals()
```

## Link Up/Down
For fault tolerance testing, it can be helpful to bring links up and down.

To disable both halves of a virtual ethernet pair:
mininet-wifi> link ap1 sta1 down

You should see an OpenFlow Port Status Change notification get generated. To bring the link back up:

mininet-wifi> link sta1 ap1 up

## Forcing Association

You can force the association with an AP either by using iw tool:
```
mininet-wifi> sta1 iw dev sta1-wlan0 connect new-ssid
```

or by using the Mininet-WiFi's API:
```
mininet-wifi> py sta1.setAssociation(ap1, intf='sta1-wlan0')
```

## Setting Tx Power
You can set the Tx Power either by iw tool (for txpower = 10):
```
mininet-wifi> sta1 iw dev sta1-wlan0 set txpower fixed 1000
```
or by using the Mininet-WiFi's API:

```
mininet-wifi> py ap1.setTxPower(10, intf='ap1-wlan1')
```

## Setting Channel
You can set the channel either by iw tool:  
if the node is AP:
```
mininet-wifi> ap1 hostapd_cli -i ap1-wlan1 chan_switch 1 2412
```
if the node is working in mesh mode:
```
mininet-wifi> sta1 iw dev sta1-mp0 set channel 1
```
if the node is working in adhoc mode:
```
mininet-wifi> sta1 iw dev sta1-wlan0 ibss leave
mininet-wifi> sta1-wlan0 ibss join adhocNet 2412 02:CA:FF:EE:BA:01
```
or by using the Mininet-WiFi's API:
```
mininet-wifi> py sta1.setChannel(6, intf='ap1-wlan1')
```

## Shutting AP down
You can shutdown the AP with:
```
mininet-wifi> py ap1.stop_()
```
and bring it up again with:

```
mininet-wifi> py ap1.start_()
```

## XTerm Display

To display an xterm for sta1 and sta2:
```
mininet-wifi> xterm sta1 sta2
```

## Building Topologies with GUI

![Branching](https://github.com/mininet-wifi/mininet-wifi.github.io/blob/master/assets/img/miniedit.png?raw=true)

You can run Miniedit from the __examples__ directory

```
~/mininet-wifi$ sudo python examples/miniedit.py
```