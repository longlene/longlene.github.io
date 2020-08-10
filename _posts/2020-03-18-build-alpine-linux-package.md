---
layout: post
title: Build Your Own Packeage for Alpine Linux
---
I wanna run Alpine Linux in VirtualBox, and port the xf86-video-vboxvideo to Alpine as xf86-video-vboxvideo is simpler than virtualbox-additions.

The following is steps to build the package.

1. Install essentail packages:
```bash
apk add alpine-sdk
abuild-keygen -a
```

2. Create user to build package
```bash
adduser va
addgroup va abuild
echo "va ALL=(ALL) ALL" >> /etc/sudoers
```

3. Fetch aports and build the package
```bash
git clone https://github.com/longlene/aports.git ~/aports
cd ~/aports/testing/xf86-video-vboxvideo
abuild -r
```

4. Add the package to system
```bash
apk add --allow-untrusted --repository /home/va/packages/testing xf86-video-vboxvideo
```

https://wiki.alpinelinux.org/wiki/Creating_an_Alpine_package 
