# 一、安装和初始化 
```
你可能还需要安装 yarn
# Mac安装yarn

$ brew install yarn
```

```
# 我们需要在命令行中安装 vue-cli 工具

$ npm install -g @vue/cli
# OR
$ yarn global add @vue/cli
```

# 二、新建项目
```
$ vue create antd-demo
# 工具会自动初始化一个脚手架并安装 Vue 项目的各种必要依赖.如果在过程中出现网络问题，请尝试配置代理或使用其他 npm registry。

# 然后我们进入项目并启动。

$ cd antd-demo
$ npm run serve

# 此时浏览器会访问 http://localhost:8080/ ，看到 Welcome to Your Vue.js App 的界面就算成功了。
```

# 三、引入 antd
```
# 这是 vue-cli 生成的默认目录结构。

├── README.md
├── babel.config
├── package.json
├── public
│   ├── favicon.ico
│   └── index.html
├── src
│   ├── assets
│   │   └── logo.png
│   ├── components
│   │   └── HelloWorld.vue
│   ├── App.vue
│   └── main.js
└── yarn.lock

# 现在从 yarn 或 npm 安装并引入 ant-design-vue。

yarn add ant-design-vue

npm i --save ant-design-vue

# 修改 src/main.js，引入 antd 的按钮组件以及全部样式文件。

import Vue from "vue";
import Button from "ant-design-vue/lib/button";
import "ant-design-vue/dist/antd.css";
import App from "./App";

Vue.component(Button.name, Button);

Vue.config.productionTip = false;

new Vue({
  render: h => h(App)
}).$mount("#app");

# 修改 src/App.vue的 template 内容。

<template>
  <div id="app">
    <img src="./assets/logo.png">
    <a-button type="primary">Button></a-button>
  </div>
</template>
...

# 好了，现在你应该能看到页面上已经有了 antd 的蓝色按钮组件，接下来就可以继续选用其他组件开发应用了。其他开发流程你可以参考 vue-cli 的官方文档。
```
