## 一、安装 vue
    
    # 查看版本
    npm -v
    2.3.0

    #升级 npm
    cnpm install npm -g

    # 升级或安装 cnpm
    npm install cnpm -g
    
    # 最新稳定版
    npm install vue
    
    sudo npm install --unsafe-perm

## 二、命令行工具 vue-cli

    vue-cil是vue的脚手架工具
    # 全局安装 vue-cli
    cnpm install --global vue-cli
    
    # 创建一个基于 webpack 模板的新项目
    vue init webpack my-project


## 三、 NPM install -save 和 -save-dev 傻傻分不清 
```
npm install moduleName             # 安装模块到项目目录下
 
npm install -g moduleName          # -g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。
 
npm install -save moduleName       # -save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。
 
npm install -save-dev moduleName   # -save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。
```
参考文档：

https://my.oschina.net/u/3138954/blog/2056301


https://www.cnblogs.com/limitcode/p/7906447.html   NPM install -save 和 -save-dev 傻傻分不清 
