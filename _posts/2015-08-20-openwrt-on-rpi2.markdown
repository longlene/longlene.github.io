---
layout: post
title: "OpenWrt on RPi2"
date: 2015-08-20 00:44:48 +0800
comments: true
categories:
---
fetch OpenWrt source code
```sh
git clone git://git.openwrt.org/openwrt.git openwrt_rpi2
./scripts/feeds update -a
./scripts/feeds install -a
```
configure the OpenWrt
```sh
make menuconfig
```

编辑package/kernel/mac80211/files/lib/wifi/mac80211.sh自动开启无线
