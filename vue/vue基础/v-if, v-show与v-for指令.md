## 一、v-if 指令
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
