# 一、绑定click事件，打印

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门</title>
    <script type="text/javascript" src="https://unpkg.com/vue"></script>
</head>

<body>
    <!--Vue实例中的数据,事件和方法-->

    <!--这是我们的View-->
    <div id="app">
        <div v-on:click="handleClick"> {{message}}</div>
    </div>

    <script>
        // 这是我们的Model
        var exampleData = {
            message: 'Hello World!'
        }

        // 创建一个 Vue 实例或 "ViewModel"
        // 它连接 View 与 Model
        new Vue({
            el: '#app',
            data: exampleData,
            methods: {
                handleClick: function() {
                    alert(123)
                }
            }
        })

    </script>

</body>

</html>
```
# 二、缩写方式实现点击改写内容
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue入门</title>
    <script type="text/javascript" src="https://unpkg.com/vue"></script>
</head>

<body>
    <!--Vue实例中的数据,事件和方法-->

    <!--这是我们的View-->
    <div id="app">
        <div @click="handleClick"> {{ message }}</div>    --这里使用了 @click 代替了 v-on:click
    </div>

    <script>
        // 这是我们的Model
        var exampleData = {
            message: 'Hello World!'
        }

        // 创建一个 Vue 实例或 "ViewModel"
        // 它连接 View 与 Model
        new Vue({
            el: '#app',
            data: exampleData,
            methods: {
                handleClick: function() {
                    this.message = "Welcome back"
                }
            }
        })

    </script>

</body>

</html>
```
