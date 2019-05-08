


## 问题一：

由于vue对语法的限制过于严格，以至于在我第一次编译运行的时候一直编译失败，当然也包括一些警告(好长一堆，删掉一些了)：

```


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

