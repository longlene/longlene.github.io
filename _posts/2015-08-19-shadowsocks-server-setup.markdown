---
layout: post
title: "shadowsocks服务搭建 "
date: 2015-08-19 00:49:09 +0800
comments: true
categories:
---
在vps上搭建shadowsocks服务(以Debian系主机为例)
首先添加GPG公钥：
`# wget -O- http://shadowsocks.org/debian/1D27208A.gpg | apt-key add -`

然后添加软件源到/etc/apt/sources.list
deb http://shadowsocks.org/debian wheezy main

最后安装软件
`# aptitude update`
`# aptitude install shadowsocks-libev`

编辑ss服务端配置文件，启动服务
