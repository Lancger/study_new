# TodoList基础功能
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>TodoList功能开发</title>
    <script type="text/javascript" src="https://unpkg.com/vue"></script>
</head>

<body>
    <div id="app">
      <div>
        <input v-model="inputValue"/>
        <button @click="handleSubmit">提交</button>
      </div>
      <ul>
          <li v-for="(item, index) of list" :key="index">
              {{ item }}
          </li>
      </ul>
    </div>

    <script>
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
