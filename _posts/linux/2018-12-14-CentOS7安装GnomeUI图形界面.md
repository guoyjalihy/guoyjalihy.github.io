---
layout: post
title:  "CentOS7安装GnomeUI图形界面"
date:   2018-12-14 21:10:01 +0800
categories: linux
---
### 使用yum命令安装Gnome
```cmd
yum groupinstall "GNOME Desktop" "Graphical Administration Tools"
```
### 提示是否安装，输入y，等待安装完成
### 安装完成后，输入下面代码，更新运行级别
```cmd
ln -sf /lib/systemd/system/runlevel5.target /etc/systemd/system/default.target
```
### 重启主机，输入reboot
