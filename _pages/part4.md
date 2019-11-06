---
layout: default
title: "Part 4"
permalink: /part4/
author_profile: true
---

# Part 4: Supported Features

## Socket Communication

The socket communication allows you to can access methods implemented in Mininet-WiFi as well as send commands from APs, stations, cars, etc. You only need to start the socket server and access it through the socket client. Please watch the demo video for a better understanding.

### Demo Video
- [https://www.youtube.com/watch?v=k69t9Xkb0nU](https://www.youtube.com/watch?v=k69t9Xkb0nU)


## Network Address Translator (NAT)

You can add a NAT to the Mininet-WiFi network by calling _net.addNAT()_, as illustrated in the code below.

```
#!/usr/bin/python

"Example to create a Mininet topology and connect it to the internet via NAT"

from mininet.node import Controller
from mininet.log import setLogLevel, info
from mn_wifi.cli import CLI_wifi
from mn_wifi.net import Mininet_wifi


def topology():

    "Create a network."

    net = Mininet_wifi(controller=Controller)

    info("*** Creating nodes\n")
    ap1 = net.addAccessPoint('ap1', ssid='new-ssid', mode='g', channel='1', position='10,10,0')
    sta1 = net.addStation('sta1', position='10,20,0')
    c1 = net.addController('c1', controller=Controller)

    info("*** Configuring wifi nodes\n")
    net.configureWifiNodes()

    info("*** Starting network\n")
    net.build()
    net.addNAT(name='nat0', linkTo='ap1', ip='192.168.100.254').configDefault()
    c1.start()
    ap1.start([c1])

    info("*** Running CLI\n")
    CLI_wifi(net)

    info("*** Stopping network\n")
    net.stop()

    
if __name__ == '__main__':
    setLogLevel('info')
    topology()
```

According to the code below, _addNAT_ creates a Node named _nat0_ linked with _ap1_. The IP 192.168.100.254 will be assigned to _nat0_ and this is the default gateway assigned to the all nodes that make up the network topology (only _sta1_ in our case).   

```
net.addNAT(name='nat0', linkTo='ap1', ip='192.168.100.254').configDefault()
```

## SUMO

![Branching](https://github.com/mininet-wifi/mininet-wifi.github.io/blob/master/assets/img/sumo.png?raw=true)

### Demo Video
- [https://www.youtube.com/watch?v=nywoltaRVSE](https://www.youtube.com/watch?v=nywoltaRVSE)

## Running P4 Tutorials

First of all you need to download the P4 VM - you may want to refer to https://github.com/p4lang/tutorials.

Once you have started the VM, install Mininet-WiFi with the following commands:
```
~$ sudo apt update
~$ git clone https://github.com/intrig-unicamp/mininet-wifi
~$ cd mininet-wifi
~/mininet-wifi$ sudo util/install.sh -Wlnfv
```
Now, you need to remove the tutorials directory
```
~$ sudo rm -r tutorials
```
and clone the tutorials repository from https://github.com/ramonfontes/tutorials
```
~$ git clone https://github.com/ramonfontes/tutorials
```
Finally, you can run any P4 tutorial from Mininet-WiFi
```
~$ cd tutorials/exercises/basic-wifi
~/tutorials/exercises/basic-wifi$ make run
```

### Demo Video

- [https://www.youtube.com/watch?v=v-_gQ7I4RXc](https://www.youtube.com/watch?v=v-_gQ7I4RXc)

* * *

## Containernet
Containernet is a fork of Mininet that allows to use Docker containers as Mininet hosts as well as Mininet-WiFi stations.
This enables interesting functionalities to built networking/cloud testbeds. The integration is done by subclassing the original Host/Station classes.


### Get started with Containernet and Mininet-WiFi

Follow these below for installing Containernet with Wi-Fi capabilities:

Clone the containernet repository from https://github.com/ramonfontes/containernet
```
~$ sudo apt-get install ansible git aptitude
~$ git clone https://github.com/ramonfontes/containernet.git
~$ cd containernet/ansible
~/containernet/ansible$ sudo ansible-playbook -i "localhost," -c local install.yml
~$ cd ..
~/containernet$ sudo python setup.py install
```
Then, you can run containernet_wifi.py
```
~/containernet$ sudo python examples/containernet_wifi.py
```
### Demo Video:

- [https://www.youtube.com/watch?v=3iL9P9T0UdQ](https://www.youtube.com/watch?v=3iL9P9T0UdQ)