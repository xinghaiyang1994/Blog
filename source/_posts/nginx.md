---
title: Nginx
categories: [Nginx]
tags: [Nginx]
date: 2017-04-25 14:55:39
updated:
---

# Nginx
Nginx (engine x) 是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器。

## Nginx 安装
安装前需要升级 yum 版本 `yum -y update` ，安装相关程序 `yum -y install gcc gcc-c++ make libtool zlib zlib-devel openssl openssl-devel pcre pcre-devel wget unzip`

* yum 安装 Nginx
    ```
    yum -y install nginx
    ```

* 源码包安装 Nginx

    1. 下载 nginx 压缩文件
    ```
    wget http://nginx.org/download/nginx-1.10.1.tar.gz      /*一般把压缩包放在 /usr/local/src 或者 /root/ 目录下,版本选择稳定版*/
    ```
    2. 解压缩
    ```
    tar -zxvf nginx-1.10.1.tar.gz
    ```
    3. 进入解压目录进行安装
    ```
    cd nginx-1.10.1     /*进入压缩目录*/
    
    ./configure --prefix=/usr/local/webserver/nginx --with-http_stub_status_module --with-http_ssl_module --with-pcre
    
    make && make install
    ```
    编译选项说明:  
    nginx大部分常用模块，编译时 `./configure --help`以 --without 开头的都默认安装。
    
        * `--prefix=PATH`  
        指定 nginx 的安装目录。默认 `/usr/local/nginx`。
        
        * `--conf-path=PATH`    
    设置nginx.conf配置文件的路径。nginx允许使用不同的配置文件启动，通过命令行中的-c选项。默认为 `conf/nginx.conf` 。
    
        * `--user=name`  
    设置nginx工作进程的用户。安装完成后，可以随时在nginx.conf配置文件更改user指令。默认的用户名是nobody。`--group=name`类似。

        * `--with-pcre`    
    设置PCRE库的源码路径，如果已通过yum方式安装，使用 `--with-pcre` 自动找到库文件。使用 `--with-pcre=PATH` 时，需要从PCRE网站下载pcre库的源码（8.39）并解压，剩下的就交给Nginx的 `./configure` 和 `make` 来完成。perl正则表达式使用在location指令和 ngx_http_rewrite_module模块中。

        * `--with-zlib=PATH`   
    指定 zlib（版本1.1.3 - 1.2.5）的源码解压目录。在默认就启用的网络传输压缩模块 ngx_http_gzip_module 时需要使用zlib 。

        * `--with-http_ssl_module`  
    使用https协议模块。默认情况下，该模块没有被构建。前提是 openssl 与 openssl-devel 已安装。

        * `--with-http_stub_status_module`  
    用来监控 Nginx 的当前状态。

        * `--with-http_realip_module`   
    通过这个模块允许我们改变客户端请求头中客户端IP地址值(例如X-Real-IP 或 X-Forwarded-For)，意义在于能够使得后台服务器记录原始客户端的IP地址。

        * `--add-module=PATH`   
    添加第三方外部模块，如nginx-sticky-module-ng或缓存模块。每次添加新的模块都要重新编译（Tengine可以在新加入module时无需重新编译）。

    4. 启动  
    使用源码包安装 nginx ，启动时需要使用绝对路径来执行。可以通过alias命令设置快捷命令。
    ```
    /usr/local/webserver/nginx/sbin/nginx       /*启动 nginx 服务*/
    
    /usr/local/webserver/nginx/sbin/nginx -s stop       /*停止 nginx 服务*/
    
    ps -aux | grep nginx        /*查看进程*/
    ```

## Nginx 常用命令
* 查看nginx进程  
    ```
    ps -ef|grep nginx    
    ```
    
* 启动nginx
    ```
    nginx
    ```
    
* 平滑启动nginx   
    平滑启动的意思是在不停止nginx的情况下，重启nginx，重新加载配置文件，启动新的工作线程，完美停止旧的工作线程。
    ```
    nginx -s reload 
    /*或者*/
    kill -HUP 'cat /var/run/nginx.pid'     /*其中/var/run/nginx.pid是nginx.pid的路径*/
    ```
    
* 停止nginx  
stop是快速停止nginx，可能并不保存相关信息。
    ```
    nginx -s stop        
    ```
    
* 完美停止nginx   
完整有序的停止nginx，并保存相关信息。
    ```
    kill -QUIT `cat /var/run/nginx.pid`
    ```
    
* 强制停止nginx  
    ```
    pkill -9 nginx
    ```
    
* 检查对nginx.conf文件的修改是否正确
    ```
    nginx -t 
    /*或者*/
    nginx -t -c nginx.conf的绝对路径
    ```
    
## nginx 作为系统服务管理的命令
将 nginx 作为系统服务管理，下载 nginx 到/etc/init.d/，修改里面的路径然后赋予可执行权限。
```
service nginx {start|stop|status|restart|reload|configtest} 
```
