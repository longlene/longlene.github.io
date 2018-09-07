---
layout: post
title: "Funtoo on RPi2"
date: 2015-04-08 23:34:31 +0800
comments: true
categories:
---
The steps of installing Funtoo on RPi2, as a checklist for myself

1. setup the filesystem
```sh
mkfs.vfat -F 16 /dev/sdf1
mkswap /dev/sdf2
mkfs.ext4 /dev/sdf3
mount /dev/sdb3 /mnt/funtoo
```

2. download the stage3 tarballs
```sh
cd /tmp
wget http://ftp.osuosl.org/pub/funtoo/funtoo-current/arm-32bit/armv7a_neonvfpv4_hardfp/stage3-latest.tar.xz
```

3. extract the stage3 file
```sh
tar xfp stage3-latest.tar.xz -C /mnt/funtoo
```

4. prepare the /boot and /lib/modules
```sh
wget http://ftp.osuosl.org/pub/funtoo/funtoo-current/snapshots/portage-latest.tar.xz
tar xjf portage-latest.tar.xz -C /mnt/gentoo/usr
```

5. setup /etc/localtime
```sh
cd /mnt/funtoo/etc/init.d
ln -s netif.tmpl netif.eth0
```
6. create config file
- the netif file `ln -s /etc/init.d/netif.eth0 /mnt/funtoo/etc/runlevels/default/netif.eth0`
- create /mnt/funtoo/etc/portage/make.conf
- modify the /mnt/funtoo/etc/conf.d/hostname

7. setup ssh keys
copy pubkey to the rpi2

8. boot funtoo
boot the rpi2 and login the system
date MMDDHHMMCCYY

9. after boot
```sh
cd /usr/portage
git checkout funtoo.org
emerege --sync
```

10. choose the profile
```
epro list
```

11. emerge the world
```sh
emerge -avuDN @world
```


官方内核自带8192cu驱动，需要手动去掉
edit the drivers/net/wireless/Kconfig and drivers/net/wireless/Makefile

PS:内核从4.0开始已经支持8192cu比较好了，rpi官方已使用内核的驱动

`make bcm2709_defconfig`

`cp arch/arm/boot/zImage /boot/kernel-$v-funtoo`

参考链接
- [Raspberry Pi Wiki](https://www.raspberrypi.org/documentation/linux/kernel/building.md)
- [Gentoo Wiki](https://wiki.gentoo.org/wiki/Raspberry_Pi/Quick_Install_Guide)
- [https://blog.ramses-pyramidenbau.de/?p=188](https://blog.ramses-pyramidenbau.de/?p=188)

