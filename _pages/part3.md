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