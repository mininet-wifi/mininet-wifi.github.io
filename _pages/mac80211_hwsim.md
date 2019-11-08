---
layout: default
title: "mac80211_hwsim"
permalink: /mac80211_hwsim/
author_profile: true
---

# mac80211_hwsim

<a id="mac80211_hwsim"></a>
## [Changing mac80211_hwsim](#mac80211_hwsim)

Please follow the steps below if you want to modify mac80211_hwsim:   

- Check the Linux Kernel version you have:
```
~$ uname -a
Linux alpha-Inspiron-5480 4.15.0-65-generic #74-Ubuntu SMP Tue Sep 17 17:06:04 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
```

- Open mac80211_hwsim.c through your favorite web browser
```
https://github.com/torvalds/linux/blob/master/drivers/net/wireless/mac80211_hwsim.c
```
- Then you need to select the branch which matches with the kernel version you have - in my case v4.15.   

- Now you need to save both mac80211_hwsim.c and mac80211_hwsim.h and proceed with the changes in mac80211_hwsim.c on locally.

- and compile your mac80211_hwsim with a Makefile (consider the content below):    

```
obj-m += mac80211_hwsim.o

all:
        make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
        make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean

```   

- and compile mac80211_hwsim with:   

```
make
``` 

- Finally, you can load your mac80211_hwsim module in Mininet-WiFi by choosing your module just before _configureWifiNodes()_:   

```
net.setModule('./mac80211_hwsim.ko')

info("*** Configuring wifi nodes\n")
net.configureWifiNodes()
```
