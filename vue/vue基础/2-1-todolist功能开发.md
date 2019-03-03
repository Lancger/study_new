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
                    this.list.push(this.inputValue)
                    this.inputValue = ''
                  }
                }
        })
    </script>

</body>

</html>
```
