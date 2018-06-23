---
layout: post
title: "Dnsmasq on macOS"
date: 2015-11-08 12:39:57 +0800
comments: true
categories:
---
First, install the dnsmasq:
```bash
brew install dnsmasq
cp /usr/local/opt/dnsmasq/dnsmasq.conf.example /usr/local/etc/dnsmasq.conf
```

To have launchd start dnsmasq at startup:
`sudo cp -fv /usr/local/opt/dnsmasq/*.plist /Library/LaunchDaemons`
`sudo chown root /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist`

Then to load dnsmasq now:
`sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist`
