---
layout: post
title:  "nginx中的的ip_hash机制"
date:   2018-12-17 19:31:01 +0800
categories: nginx
---

### 问题描述
Nginx中的ip_hash技术能够将某个ip 的请求定向到同一台后端web机器中,这样一来这个ip 下的客户端和某个后端 web机器就能建立起稳固的session.

ip_hash机制能够让某一客户机在相当长的一段时间内只访问固定的后端的某台真实的web服务器,这样会话就会得以保持,在网站页面进行login的时候就不会在后面的web服务器之间跳来跳去了,也不会出现登录一次的网站又提醒重新登录的情况.

### 配置
```properties
upstream nginx.example.com{

    server 192.168.74.235:80;
    
    server 192.168.74.236:80;
    
    ip_hash;

}

server{

    listen 80;
    
    location /{
    
        proxy_pass
        
        http: //nginx .example.com;

    }

}
```

### ip_hash机制缺陷
- nginx不是最前端的服务器
    > ip_hash要求nginx一定是最前端的服务器,否则nginx得不到正确ip,就不能根据ip作hash.	Eg: 使用的是squid为最前端.那么nginx取ip时只能得到squid的服务器ip地址,用这个地址来作分流肯定是错乱的
- nginx的后端还有其它负载均衡    
    > 假如nginx后端还有其它负载均衡,将请求又通过另外的方式分流了,那么某个客户端的请求肯定不能定位到同一台session应用服务器上,这么算起来,nginx后端只能直接指向应用服务器,或者再搭一人squid,然后指向应用服务器. 最好 的办法是用location作一次分流,将需要session的部分请求通过ip_hash分流,剩下的走其它后端去.