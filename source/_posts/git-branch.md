---
title: Git 分支
categories: [版本控制]
tags: [Git]
date: 2017-11-02 11:27:05
updated:
---

# Git 分支
列出分支:
* 列出所有本地分支
    ```
    git branch
    ```

* 列出所有远程分支
    ```
    git branch -r
    ```

* 列出所有本地分支和远程分支
    ```
    git branch -a
    ```
新建分支:
* 新建一个分支，但仍停留在当前分支
    ```
    git branch 分支1(不用引号)     /*创建一个分支名为分支1的分支*/
    ```

* 新建一个分支，并切换到该分支
    ```
    git chechout -b 分支1(不用引号)
    ```

* 新建远程分支
    ```
    git push origin 新的远程分支名      /*git push origin dev,新建一个远程分支dev,必须在新建的本地分支上才能新建远程分支，并且本地分支名和远程分支名一致*/ 
    ``` 
    
切换分支:
* 切换到指定分支，并更新工作区
    ```
    git checkout 分支1       /*切换到分支1，分支1会把当前分支的文件覆盖*/
    ```

* 切换到上一个分支
    ```
    git checkout -    
    ``` 
    
合并分支:
* 合并指定分支到当前分支
    ```
    git merge 分支1     /*把分支1合并到当前分支*/    
    ```    
    
删除分支:
* 删除已合并的分支
    ```
    git branch -d 分支1
    ```

* 删除远程分支
    ```
    git push origin :远程分支1      /*git push origin :dev 删除远程dev分支，比新建远程分支多了个:*/
    git branch -dr 远程分支1        /*远程分支的名字是 git branch -r中显示的*/
    ```

* 删除未合并的分支
    ```
    git branch -D 分支1    
    ```    
    
本地分支与远程分支关联:
* 当前分支与远程分支关联	
```
git branch -u 远程分支      /*git branch -u origin/dev,当前分支与远程dev分支关联*/
```

* 新建分支与远程分支关联
```
git branch -t 新建分支 远程分支     /*git branch -t dev origin/dev,新建dev分支并与远程分支关联*/    
```    
    
小结:  
切分支主要是为了在原有的基础上新建一条开发线同时不影响原有分支。切换的分支每次修改时，都需要`git add .` `git commit -m "描述信息"` 两步，这是对切换的分支修改内容的保存。在主分支上合并分支`git merge 非主分支` ，非主分支的内容会将原来主分支的内容覆盖，有时也会显示出差异，需要手动修改。合并后的非主分支要删除`git branch -d 非主分支` 。    
    
    
    
    
    
    
    
    
    
    