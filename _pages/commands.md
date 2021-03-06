---
layout: default
title: "Commands"
permalink: /commands/
author_profile: true
---

<a id="commands"></a>
# [Part 3: Mininet-WiFi Command-Line Interface (CLI) Commands and Attributes](#part3)
 
 
<a id="display"></a>
## [Display Options](#display)
To see the list of Command-Line Interface (CLI) options, start up a minimal topology and leave it running. Build the Mininet-WiFi:

```
$ sudo mn --wifi
```

<a id="help"></a>
### [Display the options:](#help)
```
mininet-wifi> help
```

<a id="interpreter"></a>
## [Python Interpreter](#interpreter)
If the first phrase on the Mininet-WiFi command line is py, then that command is executed with Python. This might be useful for extending Mininet-WiFi, as well as probing its inner workings. Each station, ao, and controller has an associated Node object.

At the Mininet CLI, run:
```
mininet-wifi> py 'hello ' + 'world'
```

Print the accessible local variables:
```
mininet-wifi> py locals()
```

<a id="updown"></a>
## [Link Up/Down](#updown)
For fault tolerance testing, it can be helpful to bring links up and down.

To disassociate `sta1` from `ap1` you can run:
```
mininet-wifi> link ap1 sta1 down
```

To bring the association back up:
```
mininet-wifi> link sta1 ap1 up
```

<a id="forcing"></a>
## [Forcing Association](#forcing)

You can force the association with an AP either by using iw tool:
```
mininet-wifi> sta1 iw dev sta1-wlan0 connect new-ssid
```

or by using the Mininet-WiFi's API:
```
mininet-wifi> py sta1.setAssociation(ap1, intf='sta1-wlan0')
```

<a id="range"></a>
## [Setting Signal Range](#range)
You can set the Signal Range when the node is being created:
```
net.addStation(... range=10)
```

or at runtime:
```
mininet-wifi> py sta1.setRange(10, intf='sta1-wlan0')
```

and confirm the new value with:
```
mininet-wifi> py sta1.wintfs[0].range
```

Keep in mind that if the signal range changes, txpower will also change.

<a id="antennagain"></a>
## [Setting Antenna Gain](#antennagain)
You can set the Antenna Gain when the node is being created:
```
net.addStation(... antennaGain=10)
```

or at runtime:
```
mininet-wifi> py ap1.setAntennaGain(10, intf='ap1-wlan1')
```

and confirm the new value with:
```
mininet-wifi> py sta1.wintfs[0].antennaGain
```

<a id="txpower"></a>
## [Setting Tx Power](#txpower)

You can set the Tx Power either by iw tool (for txpower = 10):
```
mininet-wifi> sta1 iw dev sta1-wlan0 set txpower fixed 1000
```

or by using the Mininet-WiFi's API:
```
net.addStation(... txpower=10)
```

as well as at runtime:
```
mininet-wifi> py ap1.setTxPower(10, intf='ap1-wlan1')
```

Confirming the new value:
```
mininet-wifi> py ap1.wintfs[0].txpower
```

<a id="channel"></a>
## [Setting Channel](#channel)
You can set the channel either by iw tool:  
### if the node is AP:
```
mininet-wifi> ap1 hostapd_cli -i ap1-wlan1 chan_switch 1 2412
```
### if the node is working in mesh mode:
```
mininet-wifi> sta1 iw dev sta1-mp0 set channel 1
```
### if the node is working in adhoc mode:
```
mininet-wifi> sta1 iw dev sta1-wlan0 ibss leave
mininet-wifi> sta1-wlan0 ibss join adhocNet 2412 02:CA:FF:EE:BA:01
```
or by using the Mininet-WiFi's API:
```
mininet-wifi> py sta1.setChannel(1, intf='ap1-wlan1')
```

Confirming the new value:
```
mininet-wifi> py sta1.wintfs[0].channel
```

<a id="renameinterfacename"></a>
## [Renaming the Interface Name](#renameinterfacename)

You can rename the network interface name with:
```
sta1.setIntfName('newName', 0)
```

You can replace `newName` by any name and `0` by the id of the interface. For example: if the original interface is sta1-wlan0 the id should by 0 while sta1-wlan1 should be 1 and so on.

<a id="shownode"></a>
## [Showing and Hiding Nodes](#shownode)

You can hide the node with:
```
sta1.hide()
```

You can show the node again with:
```
sta1.show()
```

<a id="setcirclecolor"></a>
## [Setting Circle Color](#setcirclecolor)
You can set the signal range - circle - color with:
```
sta1.set_circle_color('r')  # for red color
```

<a id="mode"></a>
## [Setting the Operation Mode](#mode)

### Master
```
sta1.setMasterMode(intf='sta1-wlan0', ssid='ap1-ssid', channel='1', mode='g')
```

### Managed
```
ap1.setManagedMode(intf='ap1-wlan1')
```

### Adhoc
```
sta1.setAdhocMode(intf='sta1-wlan0')
```

### Mesh
```
sta1.setMeshMode(intf='sta1-wlan0')
```

<a id="position"></a>
## [Setting the Node Position](#position)
```
mininet-wifi> py sta1.setPosition('10,10,0') # x=10, y=10, z=0
```

Confirming the position:
```
mininet-wifi> py sta1.position
```

<a id="apshutdown"></a>
## [Shutting AP down](#apshutdown)
You can shutdown the AP with:
```
mininet-wifi> py ap1.stop_()
```
and bring it up again with:

```
mininet-wifi> py ap1.start_()
```

<a id="stopsimulation"></a>
## [Stopping the Simulation](#stopsimulation)
Considering that you have some simulation with mobility running you can stop it with:
```
mininet-wifi> stop
```

And run it again with:
```
mininet-wifi> start
```

<a id="xterm"></a>
## [XTerm Display](#xterm)

To display an xterm for sta1 and sta2:
```
mininet-wifi> xterm sta1 sta2
```