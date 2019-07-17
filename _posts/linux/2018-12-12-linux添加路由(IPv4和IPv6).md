---
layout: post
title:  "linux添加路由(IPv4和IPv6)"
date:   2018-12-12 20:31:01 +0800
categories: linux
---

### 一、使用 route 命令添加路由信息 
注：机器重启或网卡重启后路由会失效  
IPv4: 
```cmd
//添加到主机的路由 
# route add –host 192.168.1.11 dev eth0 
# route add –host 192.168.1.12 gw 192.168.1.1 
//添加到网络的路由 
# route add –net 192.168.1.11 netmask 255.255.255.0 eth0 
# route add –net 192.168.1.11 netmask 255.255.255.0 gw 192.168.1.1 
# route add –net 192.168.1.0/24 eth1 
//添加默认网关 
# route add default gw 192.168.2.1 
//删除路由 
# route del –host 192.168.1.11 dev eth0
```
IPv6:
```cmd
ip -6 route add 目标ip/掩码 via 本地网关
eg: ip -6 route add 9a01:600:0:1b02::ad/121 via 9A01:600:0:1B02:0:0:0:71
```
### 二、在linux下设置永久路由的方法
#### 1. IPv4
 - 方法一：在/etc/rc.local文件中添加如下代码
```cmd
#IPv4
route add -net 192.168.3.0/24 dev eth0 
route add -net 192.168.2.0/24 gw 192.168.2.254
```
 - 方法二：在/etc/sysconfig/network里添加到末尾
 ```cmd
GATEWAY=gw-ip 或者 GATEWAY=gw-dev
```
 - 方法三：修改static-routes文件 /etc/sysconfig/static-routes。（如果没有就新建一个）
```cmd
any net 192.168.3.0/24 gw 192.168.3.254 
any net 10.250.228.128 netmask 255.255.255.192 gw 10.250.228.129 
```
#### 2. IPv6
 - 方法一：/etc/sysconfig/network-scripts/ifcfg-eth0 增加
```cmd
IPV6INIT=yes
 IPV6ADDR=2001:db0::1096/64
 IPV6_DEFAULTGW=2001:db0::196
```
 - 方法二：可以编辑或添加vi /etc/sysconfig/static-routes-ipv6
```cmd
#Device IPv6 network to route IPv6 gateway address 
eth0    fec0:0:0:2::/64       fec0:0:0:1:0:0:0:20 
eth0    2000::/125            3ffe:ffff:0000:f102:0:0:0:1
#或者是添加或者编辑/etc/sysconfig/network-scripts/route6-eth0:
64:ff9b::/96 via 2001:db0::196 dev eth0 
```
### 三、重启网络
 - 方法一： service network restart
 - 方法二： ifdown eth0/ifup eth0