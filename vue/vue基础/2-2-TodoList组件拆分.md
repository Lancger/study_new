## 一、全局组件
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>TodoList功能开发</title>
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>

<body>
    <div id="app">
      <div>
        <!-- v-model数据双向绑定 -->
        <input v-model="inputValue"/>
        <button @click="handleSubmit">提交</button>
      </div>
      <ul>
          <!-- 这里考虑将li标签拆分为一个组件，通过todo-item标签来引用 -->
          <!-- <li v-for="(item, index) of list" :key="index">
              {{ item }}
          </li> -->
          <todo-item></todo-item>
      </ul>
    </div>

    <script>

        // 这里定义的一个全局组件
        Vue.component('todo-item',{
            template: '<li>item</li>'
        })

        new Vue({
            el: '#app',
            data: {
                inputValue: '',
                list: [],
            },
            methods: {
                  handleSubmit: function() {
                    this.list.push(this.inputValue)  //点击事件，将当前输入的数据插入到list列表里
                    this.inputValue = ''   //这里是为了输入框置为空
                  }
                }
        })
    </script>

</body>

</html>
```
## 二、局部组件
```

```
