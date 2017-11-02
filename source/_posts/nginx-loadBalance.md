---
title: Nginx 负载均衡
categories: [Nginx]
tags: [Nginx]
date: 2017-05-03 23:08:04
updated:
---

# Nginx 负载均衡
负载均衡就是当将大量请求均衡分发到其他多个服务器上，减少服务器的负载量。

## 基于 weight 权重的负载
nginx.conf:

    upstream one{
        server 127.0.0.1:8080 weight=10;
        server 127.0.0.1:8081 weight=10;
        server 127.0.0.1:8082 weight=10;
    }
    
    server{
        listen 80;
        server_name test.xinghaiyang.com;
        root /home/wwwroot/default/test;
        location / {
                 proxy_pass http://one;
                 proxy_set_header  X-Real-IP  $remote_addr;
        }
    }

多次访问test.xinghaiyang.com时，请求实际上是根据服务器的8080、8081、8082这三个端口的权重来访问这三个端口。

参数说明:
    
    upstream webservers{
        server 127.0.0.1:8080 down;
        server 127.0.0.1:8081 weight=10 max_fails=2 fail_timeout=30s;
        server 127.0.0.1:8082 backup;
    }
  
* weight  
表示权重

* max_fails  
允许请求失败的次数，默认为1。当超过最大次数时，返回proxy_next_upstream 模块定义的错误。

* fail_timeout  
在经历了max_fails次失败后，暂停服务的时间。max_fails可以和fail_timeout一起使用，进行健康状态检查。

* down  
表示这台机器暂时不参与负载均衡。相当于注释掉了。

* backup  
表示这台机器是备用机器，是其他的机器不能用的时候，这台机器才会被使用，俗称备胎。

## 基于 ip_hash 的负载
这种分配方式，每个请求按访问IP的hash结果分配，这样来自同一个IP的访客固定访问一个后端服务器，有效解决了动态网页存在的session共享问题。

    upstream one{
        ip_hash;
        server 127.0.0.1:8080;
        server 127.0.0.1:8081;
        server 127.0.0.1:8082;
    }
    
    server{
        listen 80;
        server_name test.xinghaiyang.com;
        root /home/wwwroot/default/test;
        location / {
                 proxy_pass http://one;
                 proxy_set_header  X-Real-IP  $remote_addr;
        }
    }

同一个ip实现多次访问test.xinghaiyang.com时，请求只会访问服务器的8080、8081、8082这三个端口中的一个。

注意:  
* ip_hash 模式下，最好不要设置weight参数，因为设置了，就相当于手动设置了，将会导致很多的流量分配不均匀。

* ip_hash模式下, backup参数不可用，加了会报错。因为本身访问就是固定的了，备用已经不管什么作用了。

