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

# 二、修改默认主应用程序组件

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

# 三、入口main.js

```
#main.js

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

# 四、父组件模板TodoList.vue

```
#TodoList.vue

<template>
<div>    <!--注意template标签中必须只能存在一组div标签，所以最外层要用一个大的div包裹住-->
  <div>
    <input v-model="inputValue" />
    <button @click="handleSubmit">提交</button>
  </div>
  <ul>
    <!-- 父组件通过@delete监听子组件触发的delete事件,一旦监听到会触发handleDelete方法 -->
    <todo-item
      v-for="(item, index) of list"
      :key="index"
      :content="item"
      :index="index"
      @delete="handleDelete"
    ></todo-item>
  </ul>
</div>
</template>

<script>
//引入TodoItem组件
import TodoItem from './components/TodoItem'

export default {
  // data: function () {
  //   return {
  //     inputValue: ''
  //   }
  // },
  // 注册TodoItem组件
  components: {
    'todo-item': TodoItem
  },
  // ES6函数，简写方式
  data () {
    return {
      inputValue: '',
      list: []
    }
  },
  methods: {
    handleSubmit () {
      //this.list 底层会做个变更，相当于this.$data.list的一个别名或者缩写，如果data中没有，会到computed中去找
      this.list.push(this.inputValue)
      //清空输入框的值
      this.inputValue = ''
    },
    handleDelete (index) {
      this.list.splice(index, 1)
    }
  }
}
</script>

<style>
</style>

```

# 五、子组件TodoItem.vue

```
#TodoItem.vue 内容

<template>
  <!-- 子组件接收属性后，这里就可以使用差值表达式来直接使用content -->
  <li @click="handleDelete">{{content}}</li>
</template>
<script>
export default {
  //子组件需要定义一个属性来接收父组件传递过来的参数
   props: ['content', 'index'],
   methods: {
     handleDelete () {
      //  alert(this.index)
      //  子组件通过$emit向外触发事件
      this.$emit('delete', this.index)
     }
   }
}
</script>
<style  scoped>
</style>

```


参考文档：

https://segmentfault.com/a/1190000015777163  vue进阶3 - Mock.js（生成随机数据，模拟Ajax请求）
