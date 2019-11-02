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

## Client Isolation

Be default, stations associated with the same access point can communicate with each other without OpenFlow rules. If you want to enable OpenFlow in such case, you need to enable the client
isolation. You can either try sudo mn --wifi --no-bridge or take examples/simplewifitopology.py as reference.  

Client isolation can be used to prevent low-level bridging of frames between associated stations in the BSS. By default, this bridging is allowed.  

You may also want to refer to the OpenFlow spec.
[https://www.opennetworking.org/images/stories/downloads/sdn-resources/onf-specifications/openflow/openflow-switch-v1.5.0.noipr.pdf
](B.6.3 IN PORT Virtual Port) 
The behavior of sending out the incoming port was not clearly defined in earlier versions of the specification. It is now forbidden unless the output port is explicitly set to OFPP_IN_PORT virtual port (0xfff8) is set. The primary place where this is used is for wireless links, where a packet is received over the wireless interface and needs to be sent to another host through the same interface. For example, if a packet needed to be sent to all interfaces on the switch, two actions would need to be specified: ”actions=output:ALL,output:IN PORT”.

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

## IEEE 802.11p

By default, mac80211_hwsim does not support IEEE 802.11p. Hence, you need to do some changes in this module in order to enable IEEE 802.11p. Please follow the steps below:

### wireless-regdb – regulatory information 

Installing all the needed dependencies:
```
sudo apt-get install python-m2crypto
pip install future
wget https://mirrors.edge.kernel.org/pub/software/network/wireless-regdb/wireless-regdb-2018.05.31.tar.gz
tar -zvxf wireless-regdb-2018.05.31.tar.gz
cd wireless-regdb-2018.05.31
```
Your wireless-regdb-2018.05.31/db.txt file must include country DE as below:

```
country DE: DFS-ETSI
        # entries 279004 and 280006
        (2402 - 2482 @ 40), (20)
        (5170 - 5250 @ 80), (20), AUTO-BW
        (5250 - 5330 @ 80), (20), DFS, AUTO-BW
        (5490 - 5710 @ 160), (27), DFS
        # 5.9ghz band
        # reference: http://www.etsi.org/deliver/etsi_en/302500_302599/302571/01.02.00_20/en_302571v010200a.pdf
        (5850 - 5870 @ 10), (30)
        (5860 - 5880 @ 10), (30)
        (5870 - 5890 @ 10), (30)
        (5880 - 5900 @ 10), (30)
        (5890 - 5910 @ 10), (30)
        (5900 - 5920 @ 10), (30)
        (5910 - 5930 @ 10), (30)
        # 60 gHz band channels 1-4, ref: Etsi En 302 567
        (57240 - 65880 @ 2160), (40), NO-OUTDOOR
```
Then run:
```
make maintainer-clean
make
sudo make install PREFIX=/
```

### CRDA - Central Regulatory Domain Agent

Install some extra packages

```
sudo apt-get install python-m2crypto libgcrypt11-dev
```
Clone the repository

```
wget https://mirrors.edge.kernel.org/pub/software/network/crda/crda-3.18.tar.gz
tar -zvxf crda-3.18.tar.gz
cd crda-3.18
```
Copy your public key (installed by wireless-regdb, see above) to CRDA sources.

```
cp /lib/crda/pubkeys/$USER.key.pub.pem pubkeys/
```

Compile and install CRDA (custom REG_BIN is needed on Debian)

```
make REG_BIN=/lib/crda/regulatory.bin
sudo make install PREFIX=/ REG_BIN=/lib/crda/regulatory.bin
```

Test CRDA and the generated regulatory.bin

```
sudo /sbin/regdbdump /lib/crda/regulatory.bin | grep -i ocb
country 00: invalid
(5850.000 - 5925.000 @ 20.000), (20.00), NO-CCK, OCB-ONLY
```

### mac80211_hwsim.c

First you have to create a Makefile with the content below:

```
obj-m += mac80211_hwsim.o
all:
      make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
      make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

Then, you should consider mac80211_hwsim with the changes below (consider to use a mac80211_hwsim
version which matches with your kernel version): You may want to refer to https://github.com/torvalds/linux/blob/master/drivers/net/wireless/mac80211_hwsim.c

``` 
static const struct ieee80211_regdomain hwsim_world_regdom_custom_01
+ .n_reg_rules = 5,
+ REG_RULE(5855-10, 5925+10, 40, 0, 33, 0),
static const struct ieee80211_regdomain hwsim_world_regdom_custom_02 = {
+ .n_reg_rules = 3,
+ REG_RULE(5855-10, 5925+10, 40, 0, 33, 0),
static const struct ieee80211_iface_combination hwsim_if_comb[] = {
+.radar_detect_widths = BIT(NL80211_CHAN_WIDTH_5) |
                            BIT(NL80211_CHAN_WIDTH_10) |
static const struct ieee80211_iface_combination hwsim_if_comb_p2p_dev[] = {
+.radar_detect_widths = BIT(NL80211_CHAN_WIDTH_5) |
                            BIT(NL80211_CHAN_WIDTH_10) |
static const struct ieee80211_channel hwsim_channels_5ghz[]
/* ITS-G5B */
+ CHAN5G(5855), /* Channel 171 */
+ CHAN5G(5860), /* Channel 172 */
+ CHAN5G(5865), /* Channel 173 */
+ CHAN5G(5870), /* Channel 174 */
+ /* ITS-G5A */
+ CHAN5G(5875), /* Channel 175 */
+ CHAN5G(5880), /* Channel 176 */
+ CHAN5G(5885), /* Channel 177 */
+ CHAN5G(5890), /* Channel 178 */
+ CHAN5G(5895), /* Channel 179 */
+ CHAN5G(5900), /* Channel 180 */
+ CHAN5G(5905), /* Channel 181 */
+ /* ITS-G5D */
+ CHAN5G(5910), /* Channel 182 */
+ CHAN5G(5915), /* Channel 183 */
+ CHAN5G(5920), /* Channel 184 */
+ CHAN5G(5925), /* Channel 185 */

#ifdef CONFIG_MAC80211_MESH
                           BIT(NL80211_IFTYPE_MESH_POINT) |
#endif
                           BIT(NL80211_IFTYPE_AP) |
                           BIT(NL80211_IFTYPE_OCB) |
                       + BIT(NL80211_IFTYPE_P2P_GO) },
if (vif->type != NL80211_IFTYPE_AP &&
            vif->type != NL80211_IFTYPE_MESH_POINT &&
          + vif->type != NL80211_IFTYPE_OCB &&
            vif->type != NL80211_IFTYPE_ADHOC)
            return;
hw->wiphy->interface_modes = BIT(NL80211_IFTYPE_STATION) |
                             BIT(NL80211_IFTYPE_AP) |
                             BIT(NL80211_IFTYPE_P2P_CLIENT) |
                             BIT(NL80211_IFTYPE_P2P_GO) |
                             BIT(NL80211_IFTYPE_ADHOC) |
                           + BIT(NL80211_IFTYPE_OCB) |
                             BIT(NL80211_IFTYPE_MESH_POINT);
```

Finally, you run the make command to compile mac80211_hwsim.


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
