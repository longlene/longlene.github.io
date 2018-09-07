---
layout: post
title: "Funtoo Quick Install List"
date: 2015-08-20 00:52:13 +0800
comments: true
categories:
---
Funtoo Quick Install List
1. Boot
boot from Live USB(Gentoo or SystemRescue CD)

2. Prepare the disk
```sh
# gdisk /dev/sda
# mkfs.ext4 /dev/sda1
# mkswap /dev/sda2
# swapon /dev/sda2
# mkfs.ext4 /dev/sda3
```

3. Create essentail directory
```bash
# mkdir /mnt/funtoo
# mount /dev/sda3 /mnt/funtoo
# mkdir /mnt/funtoo/Boot
# mount /dev/sda1 /mnt/funtoo/Boot
```

4. Download the stage3 file
set date at first
```bash
# date 03110024
```

download the stage file
```bash
# cd /mnt/funtoo
# wget http://build.funtoo.org/funtoo-current/x86-64bit/generic_64/stage3-late    st.tar.xz
# tar xvpf stage3-latest.tar.xz
```

5. Chroot into Funtoo
mount filesystem
```bash
# mount -t proc none proc
# mount --rbind /sys sys
# mount --rbind /dev dev
```

```bash
# cp /etc/resolv.conf etc
# env -i HOME=/root TERM=$TERM chroot . bash -l
```
sync the 
```bash
# emerge --sync
```
edit the fstab file
```bash
nano -w /etc/fstab
```
```bash
# ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

6. kernal installation
```bash
# mkdir /etc/portage/sets
# echo sys-kernel/gentoo-sources >> /etc/portage/sets/kernel
# emerge -u @kernel
```

compile the kernel
```sh
# cd /usr/src/linux
# make menuconfig
# make && make modules_install
# cp -v arch/x86_64/boot/bzImage /boot/kernel-xxx-gentoo
```

7. boot-loader installation
```sh
# echo sys-boot/boot-update >> /etc/portage/sets/tiny
# emerge -u @tiny
# nano -w /etc/boot.conf
# grub-install /dev/sda
# boot-update
```

8.  add new user
`useradd -m -G users,wheel,audio -s /bin/zsh loong0`

