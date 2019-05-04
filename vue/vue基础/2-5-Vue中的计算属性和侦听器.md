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
## 二、使用计算属性fullName得出姓名
计算属性性能还是比较高的，当firstName或者lastName都没有变的时候，fullName会使用上一次的缓存结果
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

# 三、使用侦听器,监测修改
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
        <div>{{ count }}</div>
    </div>

    <script>
        new Vue({
            el: '#app',
            data: {
                firstName: "",
                lastName: "",
                count: 0
            },
            computed: {
                fullName: function () {
                    return this.firstName + ' ' + this.lastName
                }
            },
            watch: {
                // firstName: function () {
                //     this.count ++
                // },
                // lastName: function () {
                //     this.count ++
                // }
                // 便捷方法，使用计算属性来侦听
                fullName: function () {
                    this.count ++
                }
            }
        })
    </script>
</body>

</html>
```
