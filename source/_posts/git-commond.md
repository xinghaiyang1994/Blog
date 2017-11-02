---
title: Git 常用命令
categories: [版本控制]
tags: [Git]
date: 2017-11-02 11:27:05
updated:
---

# Git 常用命令
详细教程：     
http://www.open-open.com/lib/view/open1328069733264.html

命令行：   
http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html

![](https://ww1.sinaimg.cn/large/c13b829ely1fjiegq5j21j20go04u0ua.jpg)

* Workspace 工作区

* Index 暂存区

* Repository 本地仓库

* Remote 远程仓库

## 常用命令
* 设置提交代码的用户信息(不是远程仓库的地址)
    ```
    git config --global user.name "你的名字(任意)"
    git config --global user.email "你的邮箱(任意)"
    git config -e       /*进入编辑config模式,按i进行编辑*/
    ```

* 下载一个项目和它的整个代码历史
    ```
    git clone 链接地址(没有引号)
    ```

* 代码提交到index(暂存区)
    ```
    git add .       /*添加当前目录的所有文件到暂存区*/
    git add 文件名(不用双引号) 文件名      /*添加指定文件到暂存区,例:git add 1.txt 2.txt*/
    ```

* 代码提交到本地仓库
    ```
    git commit -m "描述信息"
    git commit 文件名(不用双引号) 文件名 -m "描述信息"      /*提交指定文件到本地仓库,例:git commit 1.txt 2.txt -m "第一次修改"*/
    git commit -a -m "描述信息"     /*等于两步 git add . 和 git commit -m "描述信息"*/
    ```

* 上传本地分支到远程仓库
    ```
    git push
    git push --force        /*强行推送当前分支到远程仓库,若有冲突,则本地代替远程*/
    git push --all      /*推送所有分支到远程仓库*/
    ```

* 取回远程仓库的变化,并与本地分支合并
    ```
    git pull        /*等价于两步 git fetch 和 git merge，分别表示下载远程仓库的所有变动 和 合并指定分支到当前分支*/
    ```

* 切换分支
    ```
    git checkout 分支1(没有引号)        /*切换到分支1*/
    ```

* 查看有变更的文件
    ```
    git status      /*只要有改动就会记录，而不是只对比变化，显示修改过的文件*/
    ```

* 显示index和工作区的差异
    ```
    get diff        /*只对比变化，显示具体修改内容*/
    ```

* 查看日志
    ```
    git log     /*显示当前分支的版本历史*/
    ```

* 恢复文件
    ```
    git checkout .       /*恢复暂存区(index区)的所有文件到工作区，就是将还未git add .的回复成上一次git add .的状态*/
    git checkout 文件名(没有引号)        /*恢复暂存区(index区)的指定文件*/
    git reset --herd        /*重置暂存区与工作区，与上一次commit保持一致。就是将git add .后的但是还未git commit的文件还原成上一次commit的状态*/
    ```

* 版本回退
    ```
    git revert --hard commit版本号        /*版本号没必要写全，前几位就可以了，Git会自动去找。commit版本号可以根据git log查看*/
    ```

* 显示所有远程仓库
    ```
    git remote -v
    ```

* 查看分支的操作
    ```
    git reflog      /*可以查看所有分支的所有操作记录（包括commit和reset的操作），可以查到commit版本号*/
    ```

