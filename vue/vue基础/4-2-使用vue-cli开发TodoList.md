# 一、项目结构
```
.
├── build/                      # webpack配置文件
│   └── ...
├── config/
│   ├── index.js                # 主要项目配置
│   └── ...
├── src/
│   ├── main.js                 # 应用入口js文件
│   ├── App.vue                 # 主应用程序组件
│   ├── components/             # 公共组件目录
│   │   └── ...
│   └── router/                 # 前端路由
│   │   └── ...
│   └── assets/                 # 模块资源（由webpack处理）
│       └── ...
├── static/                     # 纯静态资源（直接复制）
├── .babelrc                    # babel 配置，es6需要babel编译
├── .postcssrc.js               # postcss 配置
├── .eslintrc.js                # eslint 配置
├── .editorconfig               # 编辑器 配置
├── .gitignore                  # 过滤无需上传的文件
├── index.html                  # index.html模板
└── package.json                # 构建脚本和依赖关系

```

# 一、修改默认主应用程序组件

App.vue 修改为 TodoList.vue

```
<template>
  <div>
    <div>
      <input v-model="inputValue" />
      <button @click="handleSubmit">提交</button>
    </div>
    <div>
      <todo-item></todo-item>
      <todo-item></todo-item>
      <todo-item></todo-item>
    </div>
  </div>
</template>
```
# 二、封装todoitem组件


# 三、父子组件传值


# 四、属性接收


参考文档：

https://segmentfault.com/a/1190000015777163  vue进阶3 - Mock.js（生成随机数据，模拟Ajax请求）
