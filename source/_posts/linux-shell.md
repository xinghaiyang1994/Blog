---
title: Linux 命令行
categories: [Linux]
tags: [Linux,命令行]
date: 2017-04-06 13:58:52
updated:
---

# Linux 命令行
Linux 命令行网址:  
http://billie66.github.io/TLCL/book/

* 键盘操作
    * tab按键  
    命令补全。

    * crtl + c  
    让当前程序停掉。

    * ctrl + d  
    退出系统，或者命令行exit。

* 切换
    * `cd`  
    回到根目录/root。

    * `cd ~`  
    回到根目录/root。

    * `cd ..`  
    上层目录。  
    一个.代表当前目录，两个点代表上层目录。
    
    * `cd -`  
    切换到上一步的目录。

* 查看当前目录
    * `pwd`  
    print working directory,显示当前文件所在目录。

* 显示当前目录下的文件及文件夹
    * `ls`  
    列出当前文件夹下的文件或目录。

    * `ls -a`  
    列出当前文件夹下的文件或目录，包括隐藏的。
    
    * `ls -l`  
    列出当前文件夹下的文件详细信息。

    * `ls -l 文件`  
    查看文件权限。
    
    * `ls -ld 文件夹`  
    查看文件夹权限。

* 拷贝(粘贴复制)
    * `cp file1 file2`  
    拷贝文件,将file1粘贴复制到file2。

    * `cp -r dir1 dir2`  
    拷贝文件夹。

* 移动、重命名文件
    * `mv 文件 位置`  
    将文件或者文件夹移动到相应位置。如果不是位置而是文件名则表示重命名。

* 删除
    * `rm 文件`  
    删除文件。

    * `rm -f 文件`  
    强制删除文件。

    * `rm -r 文件夹`  
    删除文件夹。

* 创建文件夹
    * `mkdir 文件夹`  
    make directory,创建文件夹。

    * `mkdir .文件夹`  
    创建隐藏文件夹。

    * `mkdir -p a1/a2/a3`  
    创建一系列空文件夹。

* 创建文件
    * `touch 文件`  
    创建文件。

    * `vim 文件`  
    创建文件，并进入该文件。

    * `touch .文件`  
    创建隐藏文件。
    
    * `> 文件`  
    创建文件，跟touch命令一样。

* 查找文件
    * `find / -name a`  
    在根目录下查找文件名为a的文件。

* vim
    * `vim 文件名`  
    开始编辑文件。

	vim三种模式：  
	* 命令模式
	* 插入模式
	* 编辑模式  

    使用ESC或i：来切换模式。
    ```
	:q      退出
	:q!     强制退出
	:wq     保存并退出
    ```
    
* 终端打印
    * `echo 相关字符`  
    打印相关字符。

* 压缩解压
    * `unzip 文件`  
    解压文件，初始使用 `yum -y install unzip` 安装。

    * `zip -r 文件夹`  
    压缩文件，初始使用 `yum -y install zip` 安装。

    * `gzip	a.txt`  
    压缩a.txt文件。

    * `gzip	-d a.txt`  
    解压a.txt文件。

    * `zcat	a.txt.gz`  
    查看压缩文件。

* 重定向(文件的覆盖追加)
    * `cat 文件`  
    查看文件,查看文件不推荐使用cat,推荐使用vim。

    * `cat file1 > file2`  
    将file1中的文件内容拷贝到file2,file2之前的文件内容会被删除。

    * `cat file1 file2> file2`  
    将file1中的文件内容追加到file2,file2之前的文件内容会被保留。

    * `cat file1 >> file2`  
    将file1中的文件内容追加到file2,file2之前的文件内容会被保留。

* 权限更改
    * `chmod 数数数 文件`   
    更改文件权限，数为八进制数，例:110为二进制，1代表有权限，0代表没权限，110转换成十进制为6，6转换成八进制为6。
    
    * `chmod 用户简称+权限字母 文件`  
    增加文件权限，其中用户简称为u,g,o,a,分表代表user,group,others,all,权限字母为r,w,x。可以连续写多个用户简称，不写用户简称为全加。

    * `chmod 用户简称-权限字母 文件`   
    减少文件权限。

* 查找
    * `locate 关键字`  
    整个系统查找文件,有时滞。需要安装`yum -y install mlocate` ,然后更新系统数据库sudo updatedb。

    * `find 关键字`  
    当前目录下查找文件。

* 设置快命令
    * `alias 快捷命令="命令"`  
    注册一个快捷命令，在用户退出后就无效。  
    可以修改 `/etc/bashrc` 文件，在该文件最后添加 `alias 快捷命令="命令"` 达到每次登录都有效。若想立即生效则执行 `source ~/.bashrc`。  




















