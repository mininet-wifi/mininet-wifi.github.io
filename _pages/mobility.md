---
layout: default
title: "Mobility Models"
permalink: /mobility/
author_profile: true
---


# Mobility Models Supported

Mininet-WiFi supports the following mobility models: 
- RandomWalk
- TruncatedLevyWalk
- RandomDirection
- RandomWayPoint
- GaussMarkov
- ReferencePoint 
- TimeVariantCommunity

## How to configure mobility models?
The mobility models can be configured with the method net.startMobility() like in examples/mobilityModel.py. You may also want to consider the use of examples/mobility.py if you want a
custom mobility standard. In addition, you may want to take into account some parameters:

### Random Walk
```
sta1 = net.addStation( ..., min_x=10, max_x=20, min_y=10, max_y=20, constantDistance=1, constantVelocity=1 )
```
min_x = Minimum X position # default=0  
max_x = Maximum X position # default=100  
min_y = Minimum Y position # default=0  
max_y = Maximum Y position # default=100  
constantDistance = the value for the constant distance traveled in each step. Default is 1.0.  
constantVelocity = the value for the constant node velocity. Default is 1.0  


### Random Direction

```
sta1 = net.addStation( ..., min_x=10, max_x=20, min_y=10, max_y=20, min_v=1, max_v=2 )
```

min_x = Minimum X position # default=0  
max_x = Maximum X position # default=100  
min_y = Minimum Y position # default=0  
max_y = Maximum Y position # default=100  
min_v = Minimum value for node velocity  
max_v = Maximum value for node velocity  


### Random Way Point
```
sta1 = net.addStation( ..., min_x=10, max_x=20, min_y=10, max_y=20, min_v=1, max_v=2, min_wt=0, max_wt=100 )
```
min_x = Minimum X position # default=0  
max_x = Maximum X position # default=100  
min_y = Minimum Y position # default=0  
max_y = Maximum Y position # default=100  
min_v = Minimum value for node velocity # default=5  
max_v = Maximum value for node velocity # default=5  
min_wt = Minimum wait time # default=0  
max_wt = Maximum wait time # default=100  

### Gaus Markov

```
sta1 = net.addStation( ..., min_x=10, max_x=20, min_y=10, max_y=20 )
```

min_x = Minimum X position # default=0  
max_x = Maximum X position # default=100  
min_y = Minimum Y position # default=0  
max_y = Maximum Y position # default=100  