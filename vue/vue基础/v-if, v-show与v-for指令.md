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
## 三、v-for 指令
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
      <ul>
          <!-- <li v-for="item of list" :key="item">{{ item }}</li> -->  这里增加一个key属性，是为了更效率
          <li v-for="(item, index) of list" :key="index">{{ item }}</li> 这里使用索引来作为key值
      </ul>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                show: true,
                list: [1, 2, 3]
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
