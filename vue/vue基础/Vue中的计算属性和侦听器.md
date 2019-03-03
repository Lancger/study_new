## 一、Vue中的计算属性和侦听器
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue中的计算属性和侦听器</title>
    <script type="text/javascript" src="https://unpkg.com/vue"></script>
</head>

<body>
    <div id="app">
        姓: <input v-model="firstName"/>
        名: <input v-model="lastName"/>
        <div>{{firstName}} {{lastName}}</div>    ---比较笨的方法，把全名拼接出来
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                firstName: "",
                lastName: ""
            }
        })
    </script>
</body>

</html>
```
## 二、使用计算属性fullname得出姓名
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Vue中的计算属性和侦听器</title>
    <script type="text/javascript" src="https://unpkg.com/vue"></script>
</head>

<body>
    <div id="app">
        姓: <input v-model="firstName"/>
        名: <input v-model="lastName"/>
        <div>{{ fullName }}</div>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                firstName: "",
                lastName: ""
            },
            computed: {
                fullName: function () {
                    return this.firstName + ' ' + this.lastName
                }
            }
        })
    </script>
</body>

</html>
```
