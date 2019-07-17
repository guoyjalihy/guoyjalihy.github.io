---
layout: post
title:  "SpringBoot实现配置分离."
date:   2019-4-8 11:41:01 +0800
categories: springboot
---

#### 命令行启动指定配置文件

```cmd
将配置文件放到/usr/local/config/下(路径可随意指定)
java -jar -Xbootclasspath/a:/usr/local/config/ /usr/local/jar/xxx.jar
```

#### IDEA里指定配置文件
1. IDEA右上角打开Edit Configurations
    <figure>
        <img src="{{ site.baseurl }}/images/配置分离图一.png" alt="image">
    </figure>
    
    <figure>
        <img src="{{ site.baseurl }}/images/配置分离图二.png" alt="image">
    </figure>

2. 在VM options里加入如下配置
```properties
-DCONF_ROOT=/home/guoyanjun/Projects/git/omc/open_omc/src/main/resources/config/application.properties
```
