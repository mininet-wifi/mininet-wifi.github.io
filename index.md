---
layout: default
---

# Download/Get Started With Mininet-WiFi

## Option 1: Mininet VM Installation (easy, recommended)

Follow these steps for a VM install:

1. [Download the Mininet-WiFi VM image](https://intrig.dca.fee.unicamp.br:8840/owncloud/index.php/s/UfEWT4banmdQuH3)
1. Download and install a virtualization system. We recommend VirtualBox (free, GPL) because it is free and works on OS X, Windows, and Linux (though it’s slightly slower than VMware in our tests.) You can also use Qemu for any platform, VMware Workstation for Windows or Linux, VMware Fusion for Mac, or KVM (free, GPL) for Linux.
1. Sign up for the [mininet-wifi-discuss](https://groups.google.com/forum/#!forum/mininet-wifi-discuss) mailing list. This is the source for Mininet-WiFi support and discussion with the friendly Mininet-WiFi community. ;-)

## Option 2: Native Installation from Source

This option works well for local VM, remote EC2, and native installation. It assumes the starting point of a fresh Ubuntu (or, experimentally, Fedora) installation.

We strongly recommend more recent Ubuntu releases, because they support newer versions of Open vSwitch. (Fedora also supports recent OVS releases)

To install natively from source, first you need to get the source code:

```
git clone git://github.com/intrig-unicamp/mininet-wifi
```

Note that the above git command will check out the latest and greatest Mininet (which we recommend!)

```
cd mininet-wifi
```

Once you have the source tree, the command to install Mininet-WiFi is:

```
sudo util/install.sh -Wlnfv
```

# Part 1: Everyday Mininet-WiFi Usage

## Interact with Stations and APs

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
Display Nodes:
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


## Test connectivity between stations

Now, verify that you can ping from station 1 to station 2:
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

* * *

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

* * *

### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```



##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip


### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)
