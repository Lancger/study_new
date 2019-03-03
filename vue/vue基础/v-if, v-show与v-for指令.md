## 一、v-if 指令
v-if 指令会根据指令从dom中把标签删除或添加
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>v-if, v-show与v-for指令</title>
    <script type="text/javascript" src="https://unpkg.com/vue"></script>
</head>

<body>
    <div id="app">
      <div v-if="show">hello world</div>
      <button @click="handleClick">toggle</button>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                show: true
            },
            methods: {
                handleClick: function() {
                    this.show = !this.show
                }
            }
        })
    </script>
</body>

</html>
```
## 二、v-show 指令
v-show指令是通过改变属性来实现 style="display: none;"
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>v-if, v-show与v-for指令</title>
    <script type="text/javascript" src="https://unpkg.com/vue"></script>
</head>

<body>
    <div id="app">
      <div v-show="show">hello world</div>
      <button @click="handleClick">toggle</button>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                show: true
            },
            methods: {
                handleClick: function() {
                    this.show = !this.show
                }
            }
        })
    </script>
</body>

</html>
```
