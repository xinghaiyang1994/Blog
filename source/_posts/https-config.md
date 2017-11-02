---
title: 网站实现https
categories: [HTTP]
tags: [https]
date: 2017-11-02 11:27:05
updated:
---

# 网站实现https
1. 申请 ssl 证书  
将证书放在服务器上。

2. 配置 nginx.conf   
如果是阿里云，需要登录阿里云后台开启443安全组。
    * 基本配置
        ```
        server {
            listen 443;     /*监听443*/
            server_name localhost;      /*localhost为域名*/
            ssl on;
            root html;      /*指定目录*/
            index index.html index.htm;
            ssl_certificate   cert/公钥名称.pem;     /*公钥路径*/
            ssl_certificate_key  cert/公钥名称.key;      /*文件路径*/
            ssl_session_timeout 5m;
            ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
            ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
            ssl_prefer_server_ciphers on;
        }
        ```
    
    * 80端口重定向到443端口
        ```
        server {
            listen 80;
            server_name localhost;      /*localhost为域名*/
            rewrite ^(.*)$ https://${server_name}$1 permanent;      /*重定向到443端口*/ 
        }
        ```
3. 重启 nginx 
    ```
    nginx -t        /*检测 nginx.conf 是否正确*/
    nginx -s reload     /*平滑重启 nginx */
    ```




