# 一、安装基础环境
```
1、安装安装 vue-cli（VUE的脚手架工具）
$ npm install --global vue-cli

# 安装完成后，我们在终端中输入
$ vue -V

2、vue-cil构建项目
# 创建一个基于 webpack 的vue项目
$ vue init webpack vue-demo  // 后续按回车安装默认即可

# 进入到创建的vue项目
$ cd vue-demo

# 安装依赖
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
$ npm install
$ sudo npm install --unsafe-perm  (Mac和Linux用户权限问题，可使用此命令，正常使用npm install即可)

# 启动项目
$ npm run dev //启动成功 http://localhost:8080 即可打开测试首页

3、安装（element-ui）
推荐使用 npm 的方式安装，它能更好地和 webpack 打包工具配合使用。
$ npm i element-ui -S
```

# 二、引入Element
你可以引入整个 Element，或是根据需要仅引入部分组件。我们先介绍如何引入完整的 Element。
```
1、完整引入

在 main.js 中写入以下内容：

// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
import App from './App'
import router from './router'

Vue.config.productionTip = false
Vue.use(ElementUI)

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})

以上代码便完成了 Element 的引入。需要注意的是，样式文件需要单独引入。
```
```
2、按需引入

借助 babel-plugin-component，我们可以只引入需要的组件，以达到减小项目体积的目的。

首先，安装 babel-plugin-component：

npm install babel-plugin-component -D

然后，将 .babelrc 修改为：

{
  "presets": [["es2015", { "modules": false }]],
  "plugins": [
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]
  ]
}

然后还需要执行

npm install --save-dev babel-preset-es2015

接下来，如果你只希望引入部分组件，比如 Button 和 Select，那么需要在 main.js 中写入以下内容：

// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import { Button, Select } from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
import App from './App'
import router from './router'

Vue.config.productionTip = false
Vue.use(Button)
Vue.use(Select)

/* 或写为
Vue.component(Button.name, Button)
Vue.component(Select.name, Select)
*/

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

# 三、全局配置

在引入 Element 时，可以传入一个全局配置对象。该对象目前支持 size 与 zIndex 字段。size 用于改变组件的默认尺寸，zIndex 设置弹框的初始 z-index（默认值：2000）。按照引入 Element 的方式，具体操作如下：
```
完整引入 Element：

import Vue from 'vue';
import Element from 'element-ui';
Vue.use(Element, { size: 'small', zIndex: 3000 });

按需引入 Element：

import Vue from 'vue';
import { Button } from 'element-ui';

Vue.prototype.$ELEMENT = { size: 'small', zIndex: 3000 };
Vue.use(Button);

按照以上设置，项目中所有拥有 size 属性的组件的默认尺寸均为 'small'，弹框的初始 z-index 为 3000。
```
