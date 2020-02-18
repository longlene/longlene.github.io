---
layout: post
title: Gentoo Quick Install in Vultr
---
This a quick list for installing Gentoo into Vultr VPS.
# 1. Preparation
Navigate to the ISO Management page, upload ISO from remote
use gentoo minimal CD: https://gentoo.ussg.indiana.edu//releases/amd64/autobuilds/current-stage3-amd64/install-amd64-minimal-20200216T214502Z.iso

boot with uploaded ISO:  
select existing instance, Setting -> Custom ISO -> Attach ISO and Reboot  
or  
deploy new server, Server Type -> Upload ISO

Open console of the instance, setup the ssh server and login.
```bash
echo root:your-liveos-pass | chpasswd
/etc/init.d/ssh start
ssh root@1.2.3.4
```

# 2. Installation
Installation steps:
```bash
# create root partition
fdisk /dev/vda # `n`, then enter and enter, `w`, just only one partition

# create filesystem
mkfs.ext4 /dev/vda1

# mount /, no boot partition, no swap, just for small space
mount /dev/vda1 /mnt/gentoo

# change directory to /mnt/gentoo
cd /mnt/gentoo

# download stage3 file
wget https://gentoo.ussg.indiana.edu//releases/amd64/autobuilds/current-stage3-amd64/stage3-amd64-20200216T214502Z.tar.xz

# extract stage3
tar xpf stage3-amd64-20200216T214502Z.tar.xz

# select the best mirror
mirrorselect -i -o >> /mnt/gentoo/etc/portage/make.conf

# add repos.conf for gentoo
mkdir -p /mnt/gentoo/etc/portage/repos.conf
cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf

# copy resolv.conf for chroot network
cp -L /etc/resolv.conf /mnt/gentoo/etc/

# mount filesystem
mount -t proc proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev

# chroot into the environment
chroot /mnt/gentoo /bin/su

# sync portage tree
emerge-webrsync

# set fstab for root filesystem
echo "/dev/vda1		/		ext4		noatime		0 1" >> /etc/fstab

# add sets, you can just emerge package if you like
mkdir /etc/portage/sets
cat >> /etc/portage/sets/tiny <<EOF
sys-kernel/gentoo-kernel-bin
sys-boot/grub
EOF

# add the sets to your box
emerge -v @tiny

# configure network
ln -s /etc/init.d/net.lo /etc/init.d/net.eth0
echo 'config_eth0="dhcp"' >> /etc/conf.d/net
rc-update add net.eth0 default

# generate /boot/grub/gurb.cfg
grub-install /dev/vda
grub-mkconfig -o /boot/grub/grub.cfg

# set root password
echo root:your-pass | chpasswd
```

# 3. Extra
Default installation has no swap, you can add swapfile if need.
```bash
# create swapfile
dd if=/dev/zero of=/swapfile count=1024 bs=1M

# change permission
chmod 600 /swapfile

# use swap
mkswap /swapfile
swapon /swapfile

# add swap setting to fstab
echo "/swapfile none    swap    sw  0   0" >> /etc/fstab
```


https://www.vultr.com/docs/installing-gentoo-linux-on-a-vultr-server
https://www.vultr.com/docs/setup-swap-file-on-linux
