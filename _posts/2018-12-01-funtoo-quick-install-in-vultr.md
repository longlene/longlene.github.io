---
layout: post
title: Funtoo Quick Install in Vultr
---
This a quick list for installing Funtoo into Vultr VPS.
# 1. Preparation
Navigate to the ISO Management page, upload ISO from remote
use systemrsccd url: https://sourceforge.net/projects/systemrescuecd/files/sysresccd-x86/5.3.2/systemrescuecd-x86-5.3.2.iso

boot with uploaded ISO:  
select existing instance, Setting -> Custom ISO -> Attach ISO and Reboot  
or  
deploy new server, Server Type -> Upload ISO

Open console of the instance, set the ssh server, login into the server.
```bash
echo root:your-liveos-pass | chpasswd
ssh root@1.2.3.4
```

# 2. Installation
Installation steps:
```bash
# create partition
fdisk /dev/vda

# crate filesystem
mkfs.ext4 /dev/vda1

# create mount point
mkdir /mnt/funtoo

# mount /, no boot partition, no swap, just for small space
mount /dev/vda1 /mnt/funtoo

# setting time
date 112817442018
hwclock --systohc

# check server arch
gcc -march=native -Q --help=target | grep march

# change directory to /mnt/funtoo
cd /mnt/funtoo

# download stage3 file
#wget https://build.funtoo.org/funtoo-current/x86-64bit/generic_64/stage3-latest.tar.xz # for generic 64bit

wget https://build.funtoo.org/funtoo-current/x86-64bit/intel64-haswell/stage3-latest.tar.xz # haswell according to arch check

# extract stage3
tar xpf stage3-latest.tar.xz

# copy resolv.conf for chroot network
cp -L /etc/resolv.conf /mnt/funtoo/etc/

# mount filesystem
mount -t proc none proc
mount --rbind /sys sys
mount --rbind /dev dev

env -i HOME=/root TERM=$TERM /bin/chroot . bash -l

# sync portage tree
ego sync

# set fstab for root filesystem
echo "/dev/vda1		/		ext4		noatime		0 1" >> /etc/fstab

# create the tiny make.conf 
cat >> /etc/portage/make.conf <<EOF
CHOST="x86_64-pc-linux-gnu"
CFLAGS="-march=haswell -O2 -pipe"
CXXFLAGS="${CFLAGS}"
CPU_FLAGS="aes avx avx2 fma3 mmx mmxext popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3"

ACCEPT_KEYWORDS="~amd64"
EOF

# add sets, you can just emerge package if you like
mkdir /etc/portage/sets
cat >> /etc/portage/sets/tiny <<EOF
sys-boot/grub
EOF

# add the sets to your box
emerge -v @tiny

# grub install
grub-install --target=i386-pc --no-floppy /dev/vda

# generate /boot/grub/gurb.cfg
boot-update

# set root password
echo root:your-pass | chpasswd
```

Make sure linux, initrd path in grub.cfg is correct.  
The generated grub.cfg must contain /boot in path, or grub can not find kernel.

The following is my grub.cfg section:
```
menuentry "Funtoo Linux genkernel - kernel-debian-sources-lts-x86_64-4.9.130-2" {
  insmod part_msdos
  insmod ext2
  set root=(hostdisk//dev/vda,msdos1)
  search --no-floppy --fs-uuid --set blablabla
  linux /boot/kernel-debian-sources-lts-x86_64-4.9.130-2 real_root=/dev/vda1 rootfstype=ext4 rand_id=blablabla
  initrd /boot/initramfs-debian-sources-lts-x86_64-4.9.130-2
  set gfxpayload=keep
}
```
Here I use the kernel in Funtoo stage3, make sure build with VIRTIO-* if install kernel yourself.

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
