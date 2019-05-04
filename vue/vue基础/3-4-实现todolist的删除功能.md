在vue里面，要实现子组件和父组件之间的通信，需要通过一个发布订阅模式，子组件告诉父组件，然后父组件监听这个事件，然后触发对应的事件函数

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
          <todo-item 
          v-for="(item, index) of list"
          :key="index"
          :content="item"
          :index="index"
          >  
          </todo-item>
      </ul>
    </div>

    <script>

        // 这里定义的一个全局组件
        Vue.component('todo-item',{
            props: ['content', 'index'],   //这里使用props来接收父组件传参过来的值
            template: '<li @click="handleClick">{{ content }} {{ index }}</li>',
            methods: {
                handleClick: function(){
                    alert(index)
                }
            }
        })

        // 这里定义的一个局部组件(需要在全局组件里使用components来注册使用)
        // var TodoItem = {
        //     template:"<li>item</li>"
        // };

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
删除功能
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
          <todo-item 
          v-for="(item, index) of list"
          :key="index"
          :content="item"
          :index="index"
          @delete="handleDelete"   
          >  
          </todo-item>
          <!--父组件监听子组件往外触发的delete事件，并执行handleDelete方法-->
      </ul>
    </div>

    <script>

        // 这里定义的一个全局组件
        Vue.component('todo-item',{
            props: ['content', 'index'],   //这里使用props来接收父组件传参过来的值
            template: '<li @click="handleClick">{{ content }} {{ index }}</li>',
            methods: {
                handleClick: function(){
                    this.$emit('delete', this.index)  //子组件往外触发一个delete事件
                }
            }
        })

        // 这里定义的一个局部组件(需要在全局组件里使用components来注册使用)
        // var TodoItem = {
        //     template:"<li>item</li>"
        // };

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
                  },
                  handleDelete: function(index) {
                    alert(index)
                    this.list.splice(index, 1)
                  }
                }
        })
    </script>

</body>

</html>
```
