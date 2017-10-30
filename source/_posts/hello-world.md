---
title: Hello World
---
# npm包发布流程
1. 编写模块   
    ```
    //main.js  
    module.exports={};
    ```
    
2. 初始化包描述文件  
    ```
    npm init        /*按提示操作*/
    ```
    
3. 注册包仓库账号  
    ```
    npm adduser     /*按提示注册用户名和邮箱*/
    ```
    
4. 上传包  
在package.json所在的同级目录中 `npm publish .`               			注意publish后面有点。

5. 包权限管理
    ```
    查看包的维护人员        npm owner ls 包名
    增加包的维护人员        npm owner add 新增的用户名 包名
    删除包的维护人员        npm owner rm 要删除的用户名 包名
    ```