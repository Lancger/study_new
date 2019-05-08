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

1、App.vue 修改为 TodoList.vue

```
#App.vue原来内容

<template>
  <div id="app">
    <img src="./assets/logo.png">
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

2、App.vue 命名为 TodoList.vue之后的内容
```
#TodoList.vue的内容

<template>
  <div>
    todolist
  </div>
</template>

<script>
export default {
}
</script>

<style>
</style>
```

3、应用入口js文件默认文件

```
#main.js的内容

// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  components: { App },
  template: '<App/>'
})

```

4、应用入口js调整后内容
```
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import TodoList from './TodoList'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  components: { TodoList },    //注册的组件为TodoList
  template: '<TodoList/>'      //显示的内容也为TodoLis
})
```


# 二、正式编写模板文件

```
#TodoList.vue内容

<template>
<div>    <!--注意template标签中必须只能存在一组div标签，所以最外层要用一个大的div包裹住-->
  <div>
    <input v-model="inputValue"/>
    <button @click="handleSubmit">提交</button>
  </div>
  <ul>
  </ul>
</div>
</template>

<script>
export default {
  // data: function () {
  //   return {
  //     inputValue: ''
  //   }
  // },
  // ES6函数，简写方式
  data () {
    return {
      inputValue: ''
    }
  },
  methods: {
    handleSummit () {
      alert(123)
    }
  }
}
</script>

<style>
</style>

```

# 三、组件的修改

```
#Todoitem.vue 内容

<template>
  <!-- 子组件接收属性后，这里就可以使用差值表达式来直接使用content -->
  <li>{{ content }}</li>
</template>
<script>
export default {
  //子组件需要定义一个属性来接收父组件传递过来的参数
   props: ['content']
}
</script>
<style  scoped>

</style>

```

# 三、父子组件传值


# 四、属性接收


参考文档：

https://segmentfault.com/a/1190000015777163  vue进阶3 - Mock.js（生成随机数据，模拟Ajax请求）
