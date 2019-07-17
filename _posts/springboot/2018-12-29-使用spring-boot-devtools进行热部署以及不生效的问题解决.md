---
layout: post
title:  "使用spring-boot-devtools进行热部署以及不生效的问题解决"
date:   2018-12-29 11:00:01 +0800
categories: springboot
---

1.pom.xml引入依赖

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
```

2.File->Setting->compiler里勾选 Build Project automatically

<figure>
    <img src="{{ site.baseurl }}/images/springboot热部署图一.jpg" alt="image">
    <figcaption>
        springboot热部署图一
    </figcaption>
</figure>

3. 大部分文档只说了前两部，但是一般都不生效，还需要配置最重要的一步：  
    ctrl + shift + alt + /,选择Registry,勾上 Compiler autoMake allow when app running
        
    <figure>
        <img src="{{ site.baseurl }}/images/springboot热部署图二.jpg" alt="image">
        <figcaption>
            springboot热部署图二
        </figcaption>
    </figure>