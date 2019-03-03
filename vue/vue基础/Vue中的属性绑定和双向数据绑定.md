# 一、属性绑定
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>属性绑定和双向数据绑定</title>
    <script type="text/javascript" src="https://unpkg.com/vue"></script>
</head>

<body>
    <div id="app">
        <!-- <div title="this is hello world">hello world</div>  title 是html的一个属性 -->
        <!-- <div v-bind:title="title">hello world</div>  使用数据的对应属性绑定 -->
        <div :title="title">hello world</div>
        <!-- <div :title="'dell le ' + title ">hello world</div>  支持模板语法拼接 -->
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                title: "this is hello world"
            }
        })
    </script>
</body>

</html>
```
# 二、数据绑定
```
```
