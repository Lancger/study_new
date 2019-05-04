# 一、引入Vue.js的几种方式

https://cn.vuejs.org/v2/guide/installation.html

1、直接引入vue.js

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门</title>
    <!-- <script src="js/vue.js"></script> -->
    <script type="text/javascript" src="https://unpkg.com/vue"></script>
</head>

<body>
    <!--挂载点， 模板，实例之间的关系-->
    <div id="app"></div>
    
    <script>
        // 创建一个 Vue 实例或 "ViewModel"
        // 它连接 View 与 Model
        new Vue({
            el: '#app',
            template: '<h1>Hello {{ msg }}</h1>',
            data: {
                msg: "world"
            }
        })
    </script>

</body>

</html>
```
