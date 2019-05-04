# 一、引入Vue.js的几种方式

https://cn.vuejs.org/v2/guide/installation.html

1、直接从官网找到vue.js的开发版本或者生产版本保存到js目录下的vue.js文件

https://vuejs.org/js/vue.js

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门</title>
    <script src="js/vue.js"></script>
</head>
<body>
    <div id="app">{{ msg }}</div>
    <script>
        new Vue({
            el: '#app',
            data: {
                msg: "Hello World"
            }
        })
    </script>
</body>
</html>
```

2、cdn方式引入

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
    <div id="app">{{ msg }}</div>
    <script>
        new Vue({
            el: '#app',
            data: {
                msg: "Hello World"
            }
        })
    </script>
</body>
</html>
```
