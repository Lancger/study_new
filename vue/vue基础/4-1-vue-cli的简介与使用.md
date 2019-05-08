# 一、vue-cli脚手架工具介绍

    关于旧版本

    Vue CLI 的包名称由 vue-cli 改成了 @vue/cli。 如果你已经全局安装了旧版本的 vue-cli (1.x 或 2.x)，你需要先通过 npm uninstall vue-cli -g 或 yarn global remove vue-cli 卸载它。

# 二、安装Nodejs

    1、点击 https://nodejs.org/en/download/ 下载并安装node。
    
    2、安装成功后，在终端输入 npm -v ,会显示npm的版本信息。

# 三、安装淘宝镜像

      npm是nodejs的包管理工具，为了加快下载速度，可安装淘宝镜像，安装成功后可cnpm代替npm，在终端输入

      npm install -g cnpm --registry=https://registry.npm.taobao.org

# 四、安装Vue CLI 3

    1、Vue CLI 3 需要 Node >=8.9

    2、如果你已经全局安装了旧版本的 vue-cli (1.x 或 2.x)，你需要先通过 npm uninstall vue-cli -g 或 yarn global remove vue-cli 卸载它。

    3、全局安装@vue/cli，在终端输入

    npm install -g @vue/cli

    安装成功后，可用 vue -V 查看版本。
    

# 五、创建vue项目

```
a、使用脚手架创建项目，vue3为项目名称

vue create vue3

如下图，你会被提示选取一个 preset。
default: 适合快速创建一个新项目的原型
Manually select features: 是面向生产的项目更加需要的
    
1、选择 default

2、选择 Manually select features, 可根据自身需求进行选择配置

3、配置项目插件和功能
```

# 六、启动项目

```
进入项目目录： 
cd vue3

启动项目： 
npm run serve
npm run dev  #启动本地开发
npm run build #打包上线

Vue CLI 3去掉了2.x build和config等目录 ,通过新建 vue.config.js 来进行配置，具体可参考https://cli.vuejs.org/zh/conf...
```

# 七、旧版

```
1、使用npm安装vue-cli
npm install vue-cli -g

2、下面直接使用vue-cli初始化webpack项目
vue init webpack todolist(项目名)

vue vue init webpack todolist

⠧ downloading template
? Project name todolist
? Project description A Vue.js project
? Author Byran <Bryan@qq.com>
? Vue build standalone
? Install vue-router? Yes
? Use ESLint to lint your code? Yes
? Pick an ESLint preset Standard
? Set up unit tests No
? Setup e2e tests with Nightwatch? No
? Should we run `npm install` for you after the project has been created? (recommended) npm

   vue-cli · Generated "todolist".

3、安装依赖
npm install
cnpm install
npm run dev

4、打包上线
npm run build


5、问题处理

npm cache clean --force

rm -rf node_modules

npm install

npm start

6、报错
报错一
Error: getaddrinfo ENOTFOUND localhost
    at errnoException (dns.js:50:10)
    at GetAddrInfoReqWrap.onlookup [as oncomplete] (dns.js:92:26)
    
解决办法，主机缺少这个绑定
127.0.0.1 localhost

```
## vue技术栈开发实战

参考文档：

https://segmentfault.com/a/1190000016267443    vue进阶1-2 - 项目搭建（@vue/cli）

https://cli.vuejs.org/zh/guide/installation.html  VUE_CLI

https://my.oschina.net/u/3138954/blog/2056301   vue-ui

https://www.cnblogs.com/tjyoung/p/6832234.html   使用vue-cli脚手架工具搭建vue-webpack项目

