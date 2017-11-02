---
title: Nginx.conf 配置
categories: [Nginx]
tags: [Nginx]
date: 2017-05-02 23:50:48
updated:
---

# Nginx.conf 配置
nginx.conf配置文件主要分为六个区域：

    main                            /*全局设置*/
    events  {                       /*nginx工作模式*/
      ....
    }
    http    {                       /*http设置*/
      ....
      upstream myproject {          /*负载均衡服务器设置*/
        .....
      }
      server  {                     /*主机设置*/
        ....
        location {                  /*URL匹配*/
            ....
        }
      }
      server  {
        ....
        location {
            ....
        }
      }
      ....
    }


* main（全局设置）  
全局设置，影响其它所有部分的设置。

* events（nginx工作模式）  
指定nginx的工作模式和工作模式及连接数上限。

* http（http设置）  
负责HTTP服务器相关属性的配置。

* upstream（上游服务器设置，主要为反向代理、负载均衡相关配置）  
用于设置一系列的后端服务器，设置反向代理及后端服务器的负载均衡。

* server（主机设置）  
指定虚拟主机域名、IP和端口。

* location（URL匹配特定位置后的设置）  
用于匹配网页位置（比如，根目录“/”,“/images”,等等）。

他们之间的关系式:      server继承main，location继承server；upstream既不会继承指令也不会被继承。它有自己的特殊指令，不需要在其他地方的应用。

## main 全局配置
nginx在运行时与具体业务功能（比如http服务或者email服务代理）无关的一些参数，比如工作进程数，运行的身份等。

    user nobody nobody;
    worker_processes 2;
    error_log  /usr/local/var/log/nginx/error.log  notice;
    pid        /usr/local/var/run/nginx/nginx.pid;
    worker_rlimit_nofile 1024;

* user  
指定Nginx Worker进程运行用户以及用户组，默认由nobody账号运行。

* woker_processes  
在配置文件的顶级main部分，worker角色的工作进程的个数，master进程是接收并分配请求给worker处理。这个数值简单一点可以设置为cpu的核数grep ^processor /proc/cpuinfo | wc -l，也是 auto 值，如果开启了ssl和gzip更应该设置成与逻辑CPU数量一样甚至为2倍，可以减少I/O操作。如果nginx服务器还有其它服务，可以考虑适当减少。

* error_log  
用来定义全局错误日志文件。日志输出级别有debug、info、notice、warn、error、crit可供选择，其中，debug输出日志最为最详细，而crit输出日志最少。

* pid  
用来指定进程id的存储文件位置。

* worker_rlimit_nofile  
指定一个nginx进程可以打开的最多文件描述符数目，这里是65535，需要使用命令“ulimit -n 65535”来设置。

* worker_cpu_affinity  
也是写在main部分。在高并发情况下，通过设置cpu粘性来降低由于多CPU核切换造成的寄存器等现场重建带来的性能损耗。如worker_cpu_affinity 0001 0010 0100 1000; （四核）。

## events 工作模式
events模块来用指定nginx的工作模式和工作模式及连接数上限。

    events {
        use kqueue;     /*mac平台*/
        worker_connections  1024;
    }

* use  
用来指定Nginx的工作模式。Nginx支持的工作模式有select、poll、kqueue、epoll、rtsig和/dev/poll。其中select和poll都是标准的工作模式，kqueue和epoll是高效的工作模式，不同的是epoll用在Linux平台上，而kqueue用在BSD系统中，因为Mac基于BSD,所以Mac也得用这个模式，对于Linux系统，epoll工作模式是首选。

* worker_connections  
用于定义Nginx每个进程的最大连接数，即接收前端的最大请求数，默认是1024。最大客户端连接数由worker_processes和worker_connections决定，即Max_clients=worker_processes*worker_connections，在作为反向代理时，Max_clients变为：Max_clients = worker_processes * worker_connections/4。 
进程的最大连接数受Linux系统进程的最大打开文件数限制，在执行操作系统命令“ulimit -n 65536”后worker_connections的设置才能生效。

## http服务器
与提供http服务相关的一些配置参数。例如：是否使用keepalive啊，是否使用gzip进行压缩等。

    http{
        include       mime.types;
        default_type  application/octet-stream;
        log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                          '$status $body_bytes_sent "$http_referer" '
                          '"$http_user_agent" "$http_x_forwarded_for"';
        access_log  /usr/local/var/log/nginx/access.log  main;
        sendfile        on;
        tcp_nopush      on;
        tcp_nodelay     on;
        keepalive_timeout  10;
        #gzip  on;
        upstream myproject {
            .....
        }
        server {
            ....
        }
    }

* include  
来用设定文件的mime类型,类型在配置文件目录下的mime.type文件定义，来告诉nginx来识别文件类型。

* default_type  
设定了默认的类型为二进制流，也就是当文件类型未定义时使用这种方式，例如在没有配置asp 的locate 环境时，Nginx是不予解析的，此时，用浏览器访问asp文件就会出现下载了。

* log_format  
用于设置日志的格式，和记录哪些参数，这里设置为main，刚好用于access_log来纪录这种类型。

* access_log  
用来纪录每次的访问日志的文件地址，后面的main是日志的格式样式，对应于log_format的main。

* sendfile  
开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，减少用户空间到内核空间的上下文切换。对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。将tcp_nopush和tcp_nodelay两个指令设置为on用于防止网络阻塞。

* keepalive_timeout  
长连接超时时间，单位是秒，这个参数很敏感，涉及浏览器的种类、后端服务器的超时设置、操作系统的设置，可以另外起一片文章了。长连接请求大量小文件的时候，可以减少重建连接的开销，但假如有大文件上传，65s内没上传完成会导致失败。如果设置时间过长，用户又多，长时间保持连接会占用大量资源。

* send_timeout  
用于指定响应客户端的超时时间。这个超时仅限于两个连接活动之间的时间，如果超过这个时间，客户端没有任何活动，Nginx将会关闭连接。

* client_max_body_size  
允许客户端请求的最大单文件字节数。如果有上传较大文件，请设置它的限制值。

* client_body_buffer_size  
缓冲区代理缓冲用户端请求的最大字节数。

模块:
1. 模块http_proxy  
这个模块实现的是nginx作为反向代理服务器的功能，包括缓存功能。proxy_pass，proxy_redirect见 location 部分。

    * proxy_connect_timeout  
    nginx跟后端服务器连接超时时间(代理连接超时)。
    
    * proxy_read_timeout  
    连接成功后，与后端服务器两个成功的响应操作之间超时时间(代理接收超时)。
    
    * proxy_buffer_size  
    设置代理服务器（nginx）从后端realserver读取并保存用户头信息的缓冲区大小，默认与proxy_buffers大小相同，其实可以将这个指令值设的小一点。
    
    * proxy_buffers  
    proxy_buffers缓冲区，nginx针对单个连接缓存来自后端realserver的响应，网页平均在32k以下的话，这样设置。
    
    * proxy_busy_buffers_size  
    高负荷下缓冲大小（proxy_buffers*2）
    
    * proxy_max_temp_file_size 
    当proxy_buffers放不下后端服务器的响应内容时，会将一部分保存到硬盘的临时文件中，这个值用来设置最大临时文件大小，默认1024M，它与proxy_cache没有关系。大于这个值，将从upstream服务器传回。设置为0禁用。
    
    * proxy_temp_file_write_size  
    当缓存被代理的服务器响应到临时文件时，这个选项限制每次写临时文件的大小。proxy_temp_path（可以在编译的时候）指定写到哪那个目录。

2. 模块http_gzip  
    * gzip  
    开启gzip压缩输出，减少网络传输。

    * gzip_min_length  
    设置允许压缩的页面最小字节数，页面字节数从header头得content-length中进行获取。默认值是20。建议设置成大于1k的字节数，小于1k可能会越压越大。
    
    * gzip_buffers  
    设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。4 16k代表以16k为单位，安装原始数据大小以16k为单位的4倍申请内存。  
    
    * gzip_http_version  
    用于识别 http 协议的版本，早期的浏览器不支持 Gzip 压缩，用户就会看到乱码，所以为了支持前期版本加上了这个选项，如果你用了 Nginx 的反向代理并期望也启用 Gzip 压缩的话，由于末端通信是 http/1.0，故请设置为 1.0。
    
    * gzip_comp_level  
    gzip压缩比，1压缩比最小处理速度最快，9压缩比最大但处理速度最慢(传输快但比较消耗cpu)
    
    * gzip_types       
    匹配mime类型进行压缩，无论是否指定,”text/html”类型总是会被压缩的。
    
    * gzip_proxied    
    Nginx作为反向代理的时候启用，决定开启或者关闭后端服务器返回的结果是否压缩，匹配的前提是后端服务器必须要返回包含”Via”的 header头。
    
    * gzip_vary   
    和http头有关系，会在响应头加个 Vary: Accept-Encoding ，可以让前端的缓存服务器缓存经过gzip压缩的页面，例如，用Squid缓存经过Nginx压缩的数据。

## upstream 负载均衡服务器设置
upstream 模块负债负载均衡模块，通过一个简单的调度算法来实现客户端IP到后端服务器的负载均衡。

    upstream a{
        ip_hash;
        server 192.168.12.1:80;
        server 192.168.12.2:80 down;
        server 192.168.12.3:8080  max_fails=3  fail_timeout=20s;
        server 192.168.12.4:8080;
    }

* upstream  
指令指定了一个负载均衡器的名称 a 。这个名称可以任意指定，在后面需要的地方直接调用即可。

* ip_hash(一种负载均衡调度算法)  
Nginx的负载均衡模块目前支持4种调度算法:
    * weight 轮询（默认）。每个请求按时间顺序逐一分配到不同的后端服务器，如果后端某台服务器宕机，故障系统被自动剔除，使用户访问不受影响。weight。指定轮询权值，weight值越大，分配到的访问机率越高，主要用于后端每个服务器性能不均的情况下。
    
    * ip_hash。每个请求按访问IP的hash结果分配，这样来自同一个IP的访客固定访问一个后端服务器，有效解决了动态网页存在的session共享问题。当负载调度算法为ip_hash时，后端服务器在负载均衡调度中的状态不能是weight和backup。
    
    * air。比上面两个更加智能的负载均衡算法。此种算法可以依据页面大小和加载时间长短智能地进行负载均衡，也就是根据后端服务器的响应时间来分配请求，响应时间短的优先分配。Nginx本身是不支持fair的，如果需要使用这种调度算法，必须下载Nginx的upstream_fair模块。
    
    * url_hash。按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，可以进一步提高后端缓存服务器的效率。Nginx本身是不支持url_hash的，如果需要使用这种调度算法，必须安装Nginx 的hash软件包。 

* server  
指令指定后端服务器的IP地址和端口，同时还可以设定每个后端服务器在负载均衡调度中的状态。  
常用的状态有：
    * down，表示当前的server暂时不参与负载均衡。
    
    * backup，预留的备份机器。当其他所有的非backup机器出现故障或者忙的时候，才会请求backup机器，因此这台机器的压力最轻。
    
    * max_fails，允许请求失败的次数，默认为1。当超过最大次数时，返回pro
    
    * xy_next_upstream 模块定义的错误。
fail_timeout，在经历了max_fails次失败后，暂停服务的时间。max_fails可以和fail_timeout一起使用。

## server虚拟主机
http服务上支持若干虚拟主机。每个虚拟主机一个对应的server配置项，配置项里面包含该虚拟主机相关的配置。在提供mail服务的代理时，也可以建立若干server。每个server通过监听地址或端口来区分。

    server {
        listen       8080;
        server_name  localhost 192.168.12.10 www.yangyi.com;
        # 全局定义，如果都是这一个目录，这样定义最简单。
        root   /Users/yangyi/www;
        index  index.php index.html index.htm; 
        charset utf-8;
        access_log  usr/local/var/log/host.access.log  main;
        aerror_log  usr/local/var/log/host.error.log  error;
        ....
    }

* listen  
监听端口，默认80，小于1024的要以root启动。可以为listen *:80、listen 127.0.0.1:80等形式。

* server_name  
服务器名，用来指定IP地址或者域名，多个域名之间用空格分开，可以通过正则匹配。

* root  
表示在这整个server虚拟主机内，全部的root web根目录。注意要和locate {}下面定义的区分开来。

* index  
全局定义访问的默认首页地址。注意要和locate {}下面定义的区分开来。 

* charset  
用于设置网页的默认编码格式。 

* access_log  
用来指定此虚拟主机的访问日志存放路径，最后的main用于指定访问日志的输出格式。

模块：
1. 模块http_stream  
这个模块通过一个简单的调度算法来实现客户端IP到后端服务器的负载均衡，upstream后接负载均衡器的名字，后端realserver以 host:port options; 方式组织在 {} 中。如果后端被代理的只有一台，也可以直接写在 proxy_pass 。
    
## location
http服务中，某些特定的URL对应的一系列配置项。location /表示匹配访问根目录。location 还有一种方式就是正则匹配，开启正则匹配这样：location ~
* root  
定义服务器的默认网站根目录位置。如果locationURL匹配的是子目录或文件，root没什么作用，一般放在server指令里面或/下。

* index  
定义路径下默认访问的文件名，一般跟着root放。有先后顺序：index.php index.html index.htm，如果没有开启目录浏览权限，又找不到这些默认首页，就会报403错误。

* proxy_pass  
请求转向backend定义的服务器列表，即反向代理，对应upstream负载均衡器。也可以proxy_pass http://ip:port。

* proxy_set_header  
用来设置header头部信息参数转发。

* expires  
用来设置HTTP应答中的Expires和Cache-Control的头标时间，来告诉浏览器访问这个静态文件时，不用再去请求服务器，直接从本地缓存读取就可以了。如果静态资源改变则访问可以立刻看到更新后的变化。

    * 语法： expires [time|epoch|max|off]
    * 默认值： expires off
    * 作用域： http, server, location  
参数说明：
        * epoch 指定“Expires”的值为 1 January, 1970, 00:00:01 GMT。 
        * max 指定“Expires”的值为 31 December 2037 23:59:59 GMT，“Cache-Control”的值为10年。 
        * -1 指定“Expires”的值为 服务器当前时间 -1s,即永远过期 
        * 负数：Cache-Control: no-cache 
        * 正数或零：Cache-Control: max-age = #, # 会转换为指定时间的秒数。比如：1d、2h、3m。 
        * off 表示不修改“Expires”和“Cache-Control”的值

* rewrite  
完成URL重写。
    * 语法：rewrite regex replacement flag 
    * 默认值：none 
    * 使用字段：server, location, if  
    参数说明：
        * regex 表示用来匹配的正则 
        * replacement 表示用来替换的 
        * flag 是尾部的标记。  
        flag取值：
            * last - url重写后，马上发起一个新的请求，再次进入server块，重试location匹配，超过10次匹配不到报500错误，地址栏url不变
            * break - url重写后，直接使用当前资源，不再执行location里余下的语句，完成本次请求，地址栏url不变
            * redirect - 返回302临时重定向，url会跳转，爬虫不会更新url。
            * permanent - 返回301永久重定向。url会跳转。爬虫会更新url。
            * 为空 - URL 不会变，但是内容已经变化，也是永久性的重定向

## 访问控制 allow/deny
Nginx 的访问控制模块默认就会安装，而且写法也非常简单，可以分别有多个allow,deny，允许或禁止某个ip或ip段访问，依次满足任何一个规则就停止往下匹配。
例

    location /nginx-status {
      stub_status on;
      access_log off;
    #  auth_basic   "NginxStatus";
    #  auth_basic_user_file   /usr/local/nginx-1.6/htpasswd;
    
      allow 192.168.10.100;
      allow 172.29.73.0/24;
      deny all;
    }

我们也常用 httpd-devel 工具的 htpasswd 来为访问的路径设置登录密码：

    # htpasswd -c htpasswd admin
    New passwd:
    Re-type new password:
    Adding password for user admin
    
    # htpasswd htpasswd admin    //修改admin密码
    # htpasswd htpasswd sean    //多添加一个认证用户
   
这样就生成了默认使用CRYPT加密的密码文件。打开上面nginx-status的两行注释，重启nginx生效。

## 列出目录 autoindex 
Nginx默认是不允许列出整个目录的。如需此功能，打开nginx.conf文件，在location，server 或 http段中加入autoindex on;，另外两个参数最好也加上去:
* autoindex_exact_size  
默认为on，显示出文件的确切大小，单位是bytes。改为off后，显示出文件的大概大小，单位是kB或者MB或者GB
* autoindex_localtime  
默认为off，显示的文件时间为GMT时间。改为on后，显示的文件时间为文件的服务器时间
    

    location /images {
      root   /var/www/nginx-default/images;
      autoindex on;
      autoindex_exact_size off;
      autoindex_localtime on;
    }
