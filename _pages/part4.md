---
layout: default
title: "Part 4"
permalink: /part4/
author_profile: true
---

<a id="part4"></a>
# [Part 4: Supported Features](#part4)
 

<a id="socket"></a>
## [Socket Communication](#socket)

The socket communication allows you to can access methods implemented in Mininet-WiFi as well as send commands from APs, stations, cars, etc. You only need to start the socket server and access it through the socket client. Please watch the demo video for a better understanding.

### Demo Video
- [https://www.youtube.com/watch?v=k69t9Xkb0nU](https://www.youtube.com/watch?v=k69t9Xkb0nU)


<a id="sumo"></a>
## [SUMO](#sumo)

![Branching](https://github.com/mininet-wifi/mininet-wifi.github.io/blob/master/assets/img/sumo.png?raw=true)

### Demo Video
- [https://www.youtube.com/watch?v=nywoltaRVSE](https://www.youtube.com/watch?v=nywoltaRVSE)

<a id="p4"></a>
## [P4](#p4)

Please refer to [`examples/p4/`](https://github.com/intrig-unicamp/mininet-wifi/tree/master/examples/p4) for your convenience.

### Demo Video

- [https://www.youtube.com/watch?v=ccYRMYkosdE](https://www.youtube.com/watch?v=ccYRMYkosdE) (in Portuguese)
- [https://www.youtube.com/watch?v=v-_gQ7I4RXc](https://www.youtube.com/watch?v=v-_gQ7I4RXc)

* * *

<a id="containernet"></a>
## [Containernet](#containernet)
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