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
        <div :title="title">hello world</div>  <!--: 表示属性简写方式-->
        <!-- <div :title="'dell lee ' + title ">hello world</div>  支持模板语法拼接 -->
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
# 二、单向数据绑定
数据决定页面的显示，页面无法改变数据
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
        <input />
        <div>{{content}}</div>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                title: "this is hello world",
                content: "this is content",    <!--这里决定了页面的显示，无法改变-->
            }
        })
    </script>
</body>

</html>
```
# 三、双向数据绑定（使用v-model="content"实现数据双向绑定）
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
        <input v-model="content"/>
        <div>{{ content }}</div>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                title: "this is hello world",
                content: "this is content"
            }
        })
    </script>
</body>

</html>
```
