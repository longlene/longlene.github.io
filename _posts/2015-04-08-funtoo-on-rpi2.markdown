---
layout: post
title: "Funtoo on RPi2"
date: 2015-04-08 23:34:31 +0800
comments: true
categories:
---
The steps of installing Funtoo on RPi2

# mkfs.vfat -F 16 /dev/sdf1
# mkswap /dev/sdf2
# mkfs.ext4 /dev/sdf3
# mount /dev/sdb3 /mnt/funtoo

cd /tmp
wget http://ftp.osuosl.org/pub/funtoo/funtoo-current/arm-32bit/armv7a_neonvfpv4_hardfp/stage3-latest.tar.xz

tar xfp stage3-latest.tar.xz -C /mnt/funtoo

prepare the /boot and /lib/modules

wget http://ftp.osuosl.org/pub/funtoo/funtoo-current/snapshots/portage-latest.tar.xz
tar xjf portage-latest.tar.xz -C /mnt/gentoo/usr

setup /etc/localtime

cd /mnt/funtoo/etc/init.d
ln -s netif.tmpl netif.eth0
create /mnt/funtoo/etc/conf.d/netif.eth0
ln -s /etc/init.d/netif.eth0 /mnt/funtoo/etc/runlevels/default/netif.eth0

create /mnt/funtoo/etc/portage/make.conf

modify the /mnt/funtoo/etc/conf.d/hostname

copy the authorized_keys

boot the rpi2 and login the system
date MMDDHHMMCCYY

cd /usr/portage
git checkout funtoo.org
emerege --sync

epro list

emerge -avuDN @world


官方内核自带8192cu，需要手动去掉
drivers/net/wireless/Kconfig
drivers/net/wireless/Makefile
PS:内核从4.0开始已经支持8192cu比较好了，rpi官方已使用内核的驱动

make bcm2709_defconfig

cp arch/arm/boot/zImage /boot/kernel-$v-funtoo

https://www.raspberrypi.org/documentation/linux/kernel/building.md
https://wiki.gentoo.org/wiki/Raspberry_Pi/Quick_Install_Guide
https://blog.ramses-pyramidenbau.de/?p=188
