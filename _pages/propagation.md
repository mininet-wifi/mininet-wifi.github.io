---
layout: default
title: "Propagation Models"
permalink: /propagation/
author_profile: true
---


# Propagation Models Supported

Mininet-WiFi supports the following propagation models: 
- Friis Propagation Loss Model,
- Log-Distance Propagation Loss Model (default)
- Log-Normal Shadowing Propagation
- Loss Model, International Telecommunication Union (ITU) Propagation Loss Model
- Two-Ray Ground Propagation Loss Model.

## How to add specific Propagation Model?
You have to call the method net.setPropagationModel() like in examples/propagationModel.py.   

You might have to consider some parameters for specifics propagation models (not mandatory), for
example:

### Friis Propagation Loss Model
```
net.setPropagationModel(model="friis", sL = $int)
```
sL = system loss

### Log-Distance Propagation Loss Model
```
net.setPropagationModel(model="logDistance", sL = $int, exp = $int)
```
sL = system loss
exp = exponent

### Log-Normal Shadowing Propagation Loss Model
```
net.setPropagationModel(model="logNormalShadowing", sL=$int, exp=$int, variance=$int)
```
sL = system loss
exp = exponent
variance = gaussian variance
