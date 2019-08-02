# 一、安装vuex
```
1、安装
npm install vuex --save
或
yarn add vuex

2、依赖 Promise
npm install es6-promise --save
yarn add es6-promise   
```
Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

  ![vuex](https://github.com/Lancger/study_new/blob/master/vue/images/vuex.png)

# 二、核心概念

## 1、State
```
Vuex 使用单一状态树——是的，用一个对象就包含了全部的应用层级状态。(state)至此它便作为一个"唯一数据源"
```
```
1、#主文件引入
#vue-cource/src/main.js 

import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import Bus from './lib/bus'

Vue.config.productionTip = false
Vue.prototype.$bus = Bus

new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')

2、#store入口文件
#vue-cource/src/store/index.js

import Vue from 'vue'
import Vuex from 'vuex'
import state from './state'
import getters from './getters'
import mutations from './mutations'
import actions from './actions'
import user from './module/user'

Vue.use(Vuex)

export default new Vuex.Store({
  state,
  getters,
  mutations,
  actions,
  modules: {
    user
  }
})

3、#设置state值（可以理解为data数据源）
#vue-cource/src/store/state.js 

const state = {
  appName: 'admin'
}
export default state

4、#设置显示
#vue-cource/src/views/store.vue


<template>
  <div>
    <a-input @input="handleInput"/>
    <p>{{ inputValue }} -> lastLetter is {{ inputValueLastLetter }}</p>
    <!-- <a-show :content="inputValue"/> -->
    <p>appName: {{ appName }}, appNameWithVersion : {{ appNameWithVersion }}</p>
    <p>userName : {{ userName }}, firstLetter is : {{ firstLetter }}</p>
  </div>
</template>
<script>
import AInput from '_c/AInput.vue'
import AShow from '_c/AShow.vue'
import { mapState, mapGetters } from 'vuex'   
export default {
  name: 'store',
  data () {
    return {
      inputValue: ''
    }
  },
  components: {
    AInput,
    AShow
  },
  computed: {
  
       ...mapState([       ...展开操作符，第一种使用传入数组方式
         'appName',
         'userName'
       ])
    
    // ...mapState({       ...展开操作符，第一种使用传入对象方式
    //   appName: state => state.appName,
    //   userName: state => state.user.userName   ---这里访问模块中的属性，需要加上user
    // })
    
    ...mapState({
      userName: state => state.user.userName
    }),
    
    ...mapState('user', {                  ---或者也可以这里指定命名空间
      userName: state => state.userName
    }),
    
    ...mapGetters([
      'appNameWithVersion',
      'firstLetter'
    ]),
    
    appName () {
      return this.$store.state.appName      --- store 实例可以在全局中使用
    },
    
    // userName () {
    //   return this.$store.state.user.userName       ---这个是访问模块module中state属性，所以需要在state后加上user模块
    // },
    
    // appNameWithVersion () {
    //   return this.$store.getters.appNameWithVersion
    // },

    inputValueLastLetter () {
      return this.inputValue.substr(-1, 1)
    }
  },
  methods: {
    handleInput (val) {
      this.inputValue = val
    }
  }
}
</script>
```

  ![vue-store](https://github.com/Lancger/study_new/blob/master/vue/images/store.png)
  
### 1.2、使用密闭命名空间方式

```
1、#state数据设置开启命名空间
#vue-cource/src/store/module/user.js

const state = {
  userName: 'Lison'
}
const getters = {
  firstLetter: (state) => {
    return state.userName.substr(0, 1)
  }
}
const mutations = {
  //
}
const actions = {
  //
}

export default {
  namespaced: true,     ---这里设置开启命名空间
  getters,
  state,
  mutations,
  actions
}

2、#使用createNamespacedHelpers
#vue-cource/src/views/store.vue

<template>
  <div>
    <a-input @input="handleInput"/>
    <p>{{ inputValue }} -> lastLetter is {{ inputValueLastLetter }}</p>
    <!-- <a-show :content="inputValue"/> -->
    <p>appName: {{ appName }}, appNameWithVersion : {{ appNameWithVersion }}</p>
    <p>userName : {{ userName }}, firstLetter is : {{ firstLetter }}</p>
  </div>
</template>
<script>
import AInput from '_c/AInput.vue'
import AShow from '_c/AShow.vue'
import { createNamespacedHelpers } from 'vuex'
const { mapState } = createNamespacedHelpers('user')

export default {
  name: 'store',
  data () {
    return {
      inputValue: ''
    }
  },
  components: {
    AInput,
    AShow
  },
  computed: {
    // ...mapState({
    //   appName: state => state.appName,
    //   userName: state => state.user.userName
    // })
    ...mapState({
      userName: state => state.userName
    }),
    ...mapGetters([
      'appNameWithVersion',
      'firstLetter'
    ]),
    appName () {
      return this.$store.state.appName
    },
    // appNameWithVersion () {
    //   return this.$store.getters.appNameWithVersion
    // },
    // userName () {
    //   return this.$store.state.user.userName
    // },
    inputValueLastLetter () {
      return this.inputValue.substr(-1, 1)
    }
  },
  methods: {
    handleInput (val) {
      this.inputValue = val
    }
  }
}
</script>
```
