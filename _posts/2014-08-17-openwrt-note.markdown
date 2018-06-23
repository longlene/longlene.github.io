---
layout: post
title: "OpenWrt Notes"
date: 2014-09-16 22:01:47 +0800
comments: true
categories:
---
最近入手了个TL-WR720N无线路由，花了几天时间折腾OpenWrt，终于把网络的问题解决了，从8月16到8月19这几天的折腾终于宣告结束。

安装OpenWrt

刷完路由器后需要使用telnet登录路由器修改root密码
``bash
telnet 192.168.1.1
passwd root
``

修改密码后，后续登录就需要使用ssh方式登录路由器
``bash
ssh root@192.168.1.1
``

编译OpenWrt

1、
准备编译环境  
编译OpenWrt需要在Linux、OS X或者BSD下进行，(由于Cygwin下文件系统不区分大小写，所以不能在Windows下编译)
因为我的系统是Funtoo Linux，已有完整的编译环境，不需要额外的准备。  
如果是其他Linux发行版，需要安装git、gcc等相关编译工具。  
除准备本机编译环境外，其他地方均使用普通用户权限进行操作。

2、下载源代码  
使用git获取源代码，更新各个包
`git https://github.com/openwrt-mirror/openwrt.git` # 获取源代码
我是从github上的镜像获取源代码的，也可以从OpenWrt官方地址：git://git.openwrt.org/openwrt.git 获取源代码。  

更新包索引
默认的包索引配置在配置文件feeds.conf.default中，可以修改添加自己需要的包
src-git packages https://github.com/openwrt/packages.git  
src-git luci http://git.openwrt.org/project/luci.git  
src-git routing https://github.com/openwrt-routing/packages.git  
src-git telephony http://git.openwrt.org/feed/telephony.git  
src-git management https://github.com/openwrt-management/packages.git  
src-git mruby https://github.com/FlowGroup/openwrt-mruby.git  
src-git shadowsocks https://github.com/madeye/shadowsocks-libev.git  
src-git exOpenWrt https://github.com/black-roland/exOpenWrt.git  
src-git openwrt_plus https://github.com/sancome/openwrt-plus.git  

更新代码
`cd openwrt`  
`./scripts/feeds update -a` # 更新软件包 更新获取feeds.conf.default中列出的所有最新的包，会创建feeds目录，生成最新的包索引feeds/packages.index  
`./scripts/feeds install -a` # 给获取到的包建立软链接，会在pakcages/feeds下创建指向feeds目录下包的链接  
如果有修改feeds.conf.default，需要重新执行update和install两步才能生效

3、检查编译环境  
`make defconfig` # 检查编译环境，若满足则生成默认配置  
Funtoo Linux一般不会有错误提示，如果缺少编译工具，则会提示包缺失，安装后再执行即可。

4、配置OpenWrt  
`make prereq`  
`make menuconfig`
配置界面和Linux内核编译配置界面类似
Target System 使用默认的 Atheros AR7xxx/AR9xxx  
Subtarget 使用默认的 Generic  
Target Profile 选择我自己的路由器 TL-WR720N  

5、可选步骤: 配置  
首先是内核配置：  
如果你需要对Linux内核进行配置，执行如下命令即可：  
`make kernel_menuconfig`  

其次是应用层配置：  
默认情况下OpenWrt不启动无线，这里修改配置使初始情况下自动开启无线功能。  
编辑package/kernel/mac80211/files/lib/wifi/mac80211.sh  
修改无线的默认配置项，将无线开启，使用psk2加密，修改ssid，设置默认密码。

6、编译
`make` # 执行make命令进行编译
可以使用如下命令编译，可以打印出详细的编译信息
`make V=s` 或者 `make V=99`

7、刷路由
生成的固件在bin/下  
factory文件是用于刷带有原生系统的路由，sysupgrade是用于已经刷了openwrt的路由
将固件上传到路由器的/tmp目录，使用sysupgrade升级即可
这里执行升级操作：  
`sysupdate -n /tmp/openwrt-ar71xx-generic-tl-wr720n-v3-squashfs-sysupgrade.bin`

8、
刷完路由后，登录路由器，改密码，然后就是配置路由，联网

wireless pppoe setting
`vim /etc/config/network`
配置wan端口，
config interface 'wan'
  option ifname 'eth0'
  option proto 'pppoe'
  option username '18705156176'
  option password '321321'

wireless mac address filter
`vim /etc/config/wireless`

shadowsocks-libev  
`vim /etc/shadowsocks.json`

目前科学上网方案：
dnsmasq[ipset] shadownsocks-libev

支持nat需要安装mod-nat-extra包，也可以直接在OpenWrt编译时启动此模块

挂载U盘
安装以下包，支持U盘自动挂载（直接把这些包编译进固件亦可)
`opkg install block-mount kmod-fs-ext4 kmod-usb-ohci kmod-usb-storage`

先将/下的文件都复制到U盘的sda1分区，然后再修改/etc/config/fstab文件，增加挂载/dev/sda1到/
然后修改U盘里/etc/config/fstab，配置swap分区的开启和/home目录的挂载（必须在U盘的分区里配置，否则将不会正确挂载

`/etc/init.d/fstab enable` #开机启动fstab  
`chmod +x /etc/hotplug.d/block/10-mount`

opkg update如果报错，可能是/etc/opkg.conf某些源路径不存在，去掉即可

20140818_083300补记，整整两天都在折腾OpenWrt，后面要学的还有很多。OpenWrt本身和嵌入式比较相关，应该好好学习一下。