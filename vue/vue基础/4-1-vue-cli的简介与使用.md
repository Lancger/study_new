# 一、vue-cli脚手架工具

关于旧版本

Vue CLI 的包名称由 vue-cli 改成了 @vue/cli。 如果你已经全局安装了旧版本的 vue-cli (1.x 或 2.x)，你需要先通过 npm uninstall vue-cli -g 或 yarn global remove vue-cli 卸载它。

可以使用下列任一命令安装这个新的包：

```
npm install -g @vue/cli

# OR

yarn global add @vue/cli
```

运行以下命令来创建一个新项目：
```
vue create hello-world


Vue CLI >= 3 和旧版使用了相同的 vue 命令，所以 Vue CLI 2 (vue-cli) 被覆盖了。如果你仍然需要使用旧版本的 vue init 功能，你可以全局安装一个桥接工具：

npm install -g @vue/cli-init

# `vue init` 的运行效果将会跟 `vue-cli@2.x` 相同

vue init webpack my-project

```

# 旧版本
```
# 全局安装 vue-cli
npm install --global vue-cli

#创建一个基于 webpack 模板的新项目
vue init webpack todolist

? Project name todolist
? Project description A vue.js project
? Author xumanbai <xumanbai@onething.net>
? Vue build standalone
? Install vue-router? No
? Use ESLint to lint your code? Yes
? Pick an ESLint preset Standard
? Set up unit tests No
? Setup e2e tests with Nightwatch? No
? Should we run `npm install` for you after the project has been created? (recom
mended) npm

   vue-cli · Generated "todolist".


# Installing project dependencies ...
# ========================

⸨ ░░░░░░░░░░░░░░░░░⸩ ⠹ fetchMetadata: sill pacote range manifest for extend-shallow@^3.0.2 fetched in 2ms

#安装依赖
cd todolist
npm run dev
```

## vue技术栈开发实战

参考文档：

https://cli.vuejs.org/zh/guide/installation.html  VUE_CLI

https://my.oschina.net/u/3138954/blog/2056301   vue-ui

https://www.cnblogs.com/tjyoung/p/6832234.html   使用vue-cli脚手架工具搭建vue-webpack项目
