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
```

  ![vue-store](https://github.com/Lancger/study_new/blob/master/vue/images/store.png)
