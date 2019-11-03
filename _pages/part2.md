---
layout: default
title: "Part 2"
permalink: /part2/
author_profile: true
---

# Part 2: Advanced Startup Options

Run a Regression Test
You don’t need to drop into the CLI; Mininet-WiFi can also be used to run self-contained regression tests.

Run a regression test:

```
$ sudo mn --wifi --test pingpair
```

This command created a minimal topology, started up the OpenFlow reference controller, ran an all-pairs-ping test, and tore down both the topology and the controller.

Another useful test is iperf (give it about 10 seconds to complete):

```
$ sudo mn --wifi --test iperf
```
This command created the same Mininet-WiFi, ran an iperf server on one host, ran an iperf client on the second host, and parsed the bandwidth achieved.

The default topology is a single ap connected to two stations. You could change this to a different topo with --topo, and pass parameters for that topology’s creation. For example, to verify all-pairs ping connectivity with one ap and three stations:

Run a regression test:
```
$ sudo mn --wifi --test pingall --topo single,3
```

## Adjustable Verbosity
The default verbosity level is info, which prints what Mininet-WiFi is doing during startup and teardown. Compare this with the full debug output with the -v param:
```
$ sudo mn --wifi -v debug
...
mininet> exit
```
Lots of extra detail will print out. Now try output, a setting that prints CLI output and little else:
```
$ sudo mn --wifi -v output
mininet> exit
```

## Client Isolation

Be default, stations associated with the same access point can communicate with each other without OpenFlow rules. If you want to enable OpenFlow in such case, you need to enable the client
isolation. You can either try
``` 
sudo mn --wifi --no-bridge 
```
or take examples/simplewifitopology.py as reference.  

Client isolation can be used to prevent low-level bridging of frames between associated stations in the BSS. By default, this bridging is allowed.  

You may also want to refer to the OpenFlow spec.
[B.6.3 IN PORT Virtual Port](https://www.opennetworking.org/images/stories/downloads/sdn-resources/onf-specifications/openflow/openflow-switch-v1.5.0.noipr.pdf
) 
**The behavior of sending out the incoming port was not clearly defined in earlier versions of the specification. It is now forbidden unless the output port is explicitly set to OFPP_IN_PORT virtual port (0xfff8) is set. The primary place where this is used is for wireless links, where a packet is received over the wireless interface and needs to be sent to another host through the same interface. For example, if a packet needed to be sent to all interfaces on the switch, two actions would need to be specified: ”actions=output:ALL,output:IN PORT”.**