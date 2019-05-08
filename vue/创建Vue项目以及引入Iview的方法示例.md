# 一、创建Vue项目过程
```
vue init webpack vue-iview

? Project name vue-iview
? Project description A Vue.js project
? Author yourname <youremail@example.com>
? Vue build standalone
? Install vue-router? Yes
? Use ESLint to lint your code? Yes
? Pick an ESLint preset Standard
? Setup unit tests with Karma + Mocha? Yes
? Setup e2e tests with Nightwatch? Yes

  vue-cli · Generated "vue-iview".

  To get started:

   cd vue-iview
   npm install
   npm run dev
```

# 二、引入iview

src/main.js

```
在main.js中添加了下面3行代码

import iView from 'iview'
import 'iview/dist/styles/iview.css'  // 使用 CSS
Vue.use(iView)

```

# 三、iview 安装
```
npm install --save iview
```

# 四、使用iview 组件

创建 src/components/LoginForm.vue

官方的组件代码缩进不符合要求，需要修改
```
<template>
  <Form ref="formInline" :model="formInline" :rules="ruleInline" inline>
    <FormItem prop="user">
      <Input type="text" v-model="formInline.user" placeholder="Username">
        <Icon type="ios-person-outline" slot="prepend"></Icon>
      </Input>
    </FormItem>
    <FormItem prop="password">
      <Input type="password" v-model="formInline.password" placeholder="Password">
        <Icon type="ios-locked-outline" slot="prepend"></Icon>
      </Input>
    </FormItem>
    <FormItem>
      <Button type="primary" @click="handleSubmit('formInline')">登录</Button>
    </FormItem>
  </Form>
</template>
<script>
export default {
 data () {
  return {
   formInline: {
    user: '',
    password: ''
   },
   ruleInline: {
    user: [
     { required: true, message: '请填写用户名', trigger: 'blur' }
    ],
    password: [
     { required: true, message: '请填写密码', trigger: 'blur' },
     { type: 'string', min: 6, message: '密码长度不能小于6位', trigger: 'blur' }
    ]
   }
  }
 },
 methods: {
  handleSubmit (name) {
   this.$refs[name].validate((valid) => {
    if (valid) {
     this.$Message.success('提交成功!')
    } else {
     this.$Message.error('表单验证失败!')
    }
   })
  }
 }
}
</script>
```

src/App.vue

```
<template>
 <div id="app">
  <LoginForm></LoginForm>
 </div>
</template>

<script>
import LoginForm from './components/LoginForm'
export default {
 name: 'app',
 components: {
  'LoginForm': LoginForm
 }
}
</script>

<style>
#app {

}
</style>
```

补充：vue安装完iview后，启动项目，提示 in ./node_modules/dist/styles/iview.css 报错

## 问题一：

由于vue对语法的限制过于严格，以至于在我第一次编译运行的时候一直编译失败，当然也包括一些警告(好长一堆，删掉一些了)：

```
  ✘  http://eslint.org/docs/rules/indent                       Expected indentation of 0 spaces but found 2   
  src/components/Message.vue:46:1
    export default {
   ^
   
   Errors:
  21  http://eslint.org/docs/rules/indent
   2  http://eslint.org/docs/rules/space-before-function-paren
 
You may use special comments to disable some warnings.
Use // eslint-disable-next-line to ignore the next line.
Use /* eslint-disable */ to ignore all warnings in a file. 
```
## 解决办法

在build/webpack.base.conf.js文件中，注释或者删除掉：module->rules中有关eslint的规则

```
module: {
  rules: [
    //...(config.dev.useEslint ? [createLintingRule()] : []), // 注释或者删除
    {
      test: /\.vue$/,
      loader: 'vue-loader',
      options: vueLoaderConfig
    },
    ...
    }
  ]
}
```


参考文档：

http://www.manongjc.com/article/20422.html  创建Vue项目以及引入Iview的方法示例

