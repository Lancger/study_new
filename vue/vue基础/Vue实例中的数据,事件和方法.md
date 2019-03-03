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
    <div id="app">
        <div v-on:click="handleClick"> {{content}}</div>
    </div>

    <script>
        // 创建一个 Vue 实例或 "ViewModel"
        // 它连接 View 与 Model
        new Vue({
            el: '#app',
            data: {
                content: "world",
            },
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
