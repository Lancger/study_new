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
