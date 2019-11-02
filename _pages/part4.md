---
layout: default
title: "Part 4"
permalink: /part4/
author_profile: true
---

# Part 4: Supported Features

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