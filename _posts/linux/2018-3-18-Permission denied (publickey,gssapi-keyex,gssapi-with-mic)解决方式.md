---
layout: post
title:  "Permission denied (publickey,gssapi-keyex,gssapi-with-mic)解决方式"
date:   2018-3-18 19:31:01 +0800
categories: linux
---

当远程登录一台服务器时会报如下错：

```cmd
[root@guoyanjun ~]# ssh 192.168.122.239
Permission denied (publickey,gssapi-keyex,gssapi-with-mic)
```

解决方式如下：

```cmd
[root@c10 ~]# vi /etc/ssh/sshd_config 
#修改如下两项：
PubkeyAuthentication yes
PasswordAuthentication yes

# 重启sshd
[root@c10 ~]# systemctl restart sshd
```

