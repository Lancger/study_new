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
```
