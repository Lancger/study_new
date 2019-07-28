# 一、安装
```
npm install axios


<template>
  <div id="app">
   <p>Hello</p>
  </div>
</template>

<script>
/*
    axios请求方法: get,post,put,patch,delete

    get: 获取数据
    post: 提交数据（表单提交, 文件上传）
    put: 更新数据 （所有数据推送到后端）
    patch: 更新数据 （只将修改的数据推送）
    delete: 删除数据
*/
import axios from 'axios'

export default {
  name: 'axios',
  components: {

  },
  created () {
    // http://localhost:8080/data.json?id=12

    axios.get('/data.json', {
      params: {
        id: 12
      }
    }).then((res) => {
      console.log(res)
    })
    axios({
      method: 'get',
      url: '/data.json',
      params: {
        id: 12
      }
    }).then((res) => {
      console.log(res)
    })
  }
}
</script>
```

# 二、get post put patch样例
```
<template>
  <div id="app">
   <p>Hello</p>
  </div>
</template>

<script>
/*
    axios请求方法: get,post,put,patch,delete

    get: 获取数据
    post: 提交数据（表单提交, 文件上传）
    put: 更新数据 （所有数据推送到后端）
    patch: 更新数据 （只将修改的数据推送）
    delete: 删除数据

    post 有两种方式
    form-data 表单方式提交（图片文件上传）
    application/json 方式提交
*/
import axios from 'axios'

export default {
  name: 'axios',
  components: {

  },
  created () {
    // http://localhost:8080/data.json?id=12

    // get
    axios.get('/data.json', {
      params: {
        id: 12
      }
    }).then((res) => {
      console.log(res)
    })
    axios({
      method: 'get',
      url: '/data.json',
      params: {
        id: 12
      }
    }).then((res) => {
      console.log(res)
    })

    // post
    // application/json 方式提交
    let data = {
      id: 12
    }
    axios.post('/post', data).then((res) => {
      console.log(res)
    })
    axios({
      method: 'post',
      url: '/post',
      data: data
    }).then((res) => {
      console.log(res)
    })

    // post
    // form-data 方式提交
    let formData = new FormData()
    for (let key in data) {
      formData.append(key, data[key])
    }
    axios.post('/post', formData).then((res) => {
      console.log(res)
    })
    axios({
      method: 'post',
      url: '/post',
      data: formData
    }).then((res) => {
      console.log(res)
    })

    // put请求
    axios.put('/put', data).then((res) => {
      console.log(res)
    })

    // patch请求
    axios.patch('/patch', data).then((res) => {
      console.log(res)
    })
  }
}
</script>
```

# 三、delete不同方式样例
```
<template>
  <div id="app">
   <p>Hello</p>
  </div>
</template>

<script>
/*
    axios请求方法: get,post,put,patch,delete

    get: 获取数据
    post: 提交数据（表单提交, 文件上传）
    put: 更新数据 （所有数据推送到后端）
    patch: 更新数据 （只将修改的数据推送）
    delete: 删除数据

    post 有两种方式
    form-data 表单方式提交（图片文件上传）
    application/json 方式提交
*/
import axios from 'axios'

export default {
  name: 'axios',
  components: {

  },
  created () {
    // detele方法 (参数方式)
    axios.delte('/delete', {
      params: {
        id: 12
      }
    }).then((res) => {
      console.log(res)
    })

    // detele方法 (传值方式)
    axios.delte('/delete', {
      data: {
        id: 12
      }
    }).then((res) => {
      console.log(res)
    })
  }
}
</script>
```

# 四、并发请求
```
样例一：
<template>
  <div id="app">
   <p>Hello</p>
  </div>
</template>

<script>
/*
  //并发请求: 同时进行多个多个请求, 并统一处理返回值
  例如聊天系统,需要同时展示个人信息和好友列表，但这2个数据是不同接口返回的
*/
import axios from 'axios'

export default {
  name: 'axios',
  created () {
    // axios.get('/data.json').then((res) => {
    //   console.log(res)
    // })

    // axios.all()  axios.spread
    axios.all(
      [
        axios.get('/data.json'),
        axios.get('/city.json')
      ]
    ).then(axios.spread((dataRes, cityRes) => {
      console.log(dataRes, cityRes)
    })
    )
  }
}
</script>



样例二：
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // Both requests are now complete
}));
```

# 五、创建axios实例
```
<template>
  <div id="app">
   <p>Hello</p>
  </div>
</template>

<script>
/*
  let arr = []
  let arr = new Array()  ---这种就是new了一个数组实例

  axios 实例
  后端接口地址有多个，并且超时时长不一样
  这样的话，就可以通过封装axios实例来配置不同的接口地址和超时时长
*/
import axios from 'axios'

export default {
  name: 'axios-instance',
  created () {
    let instance1 = axios.create({
      baseURL: 'http://localhost:8080',
      timeout: 1000
    })
    let instance2 = axios.create({
      baseURL: 'http://localhost:8080',
      timeout: 5000
    })
    instance1.get('/data.json').then(res => {
      console.log(res)
    })
    instance2.get('/city.json').then(res => {
      console.log(res)
    })
  }
}
</script>
```

# 六、axios实例配置
```
<template>
  <div id="app">
   <p>Hello</p>
  </div>
</template>

<script>
/*
  axios配置参数有哪些

  优先级情况
  1.axios请求配置-->2.axios实例配置-->3.axios全局配置
*/
import axios from 'axios'

export default {
  name: 'axios-instance',
  created () {
    axios.create({
      baseURL: 'http://localhost:8080', // 请求的域名，基本地址
      timeout: 1000, // 请求超时时长
      url: '/data.json', // 请求参数
      method: 'get, post, put, patch, delete',
      headers: {
        token: ''
      }, // 请求头
      params: {}, // 请求参数拼接在url
      data: {} // 请求参数放在请求体
    })

    // 1. axios全局配置
    axios.defaults.timeout = 1000
    axios.defaults.baseURL = 'http://localhost:8090'

    // 2. axios实例配置
    let instance = axios.create()
    instance.defaults.timeout = 3000

    // 3. axios请求配置
    instance.get('/data.json', {
      timeout: 5000
    })
  }
}
</script>

```

# 七、axios常用配置方法
```
<template>
  <div id="app">
   <p>Hello</p>
  </div>
</template>

<script>
/*
  axios配置参数有哪些

  优先级情况
  1.axios请求配置-->2.axios实例配置-->3.axios全局配置
*/
import axios from 'axios'

export default {
  name: 'axios-instance',
  created () {
    axios.create({
      baseURL: 'http://localhost:8080', // 请求的域名，基本地址
      timeout: 1000, // 请求超时时长
      url: '/data.json', // 请求参数
      method: 'get, post, put, patch, delete',
      headers: {
        token: ''
      }, // 请求头
      params: {}, // 请求参数拼接在url
      data: {} // 请求参数放在请求体
    })

    // 1. axios全局配置
    axios.defaults.timeout = 1000
    axios.defaults.baseURL = 'http://localhost:8090'

    // 2. axios实例配置
    let instance = axios.create()
    instance.defaults.timeout = 3000

    // 3. axios请求配置
    instance.get('/data.json', {
      timeout: 5000
    })

    // 实际开发
    // 有两种请求接口
    // http://localhost:9090
    // http://localhost:9091
    let instance1 = axios.create({
      baseURL: 'http://localhost:9090',
      timeout: 1000
    })
    let instance2 = axios.create({
      baseURL: 'http://localhost:9091',
      timeout: 5000
    })
    instance1.get('/contactList', {
      params: { id: 12 }
    }).then((res) => {
      console.log(res)
    })
    instance2.get('/orderList', {
      timeout: 3000
    }).then((res) => {
      console.log(res)
    })
  }
}
</script>

```

# 八、拦截器
```
<template>
  <div id="app">
   <p>Hello</p>
  </div>
</template>

<script>
/*
  axios拦截器: 在请求或响应被处理前拦截它们
  1、请求拦截器
  2、响应拦截器
*/
import axios from 'axios'

export default {
  name: 'axios',
  created () {
    // 请求拦截器
    axios.interceptors.request.use(config => {
      // 在发送请求前做些什么
      return config
    }, err => {
      // 在请求错误的时候做些什么
      return Promise.reject(err)
    })

    // 响应拦截器
    axios.interceptors.response.use(res => {
      // 请求成功对响应数据做处理
      return res
    }, err => {
      // 在响应错误的时候做什么
      return Promise.reject(err)
    })

    axios.get().then().catch(err => {
      console.log(err)
    })

    // 取消拦截器 （了解）
    let interceptors = axios.interceptors.request.use(config => {
      config.headers = {
        auth: true
      }
      return config
    })
    axios.interceptors.request.eject(interceptors)

    // 例子 登录模块 （token: 'xxxxxxxx'）
    // 需要登录的接口
    let instance = axios.create({})
    instance.interceptors.request.use(config => {
      config.headers.token = ''
      return config
    })

    // 不需要登录的接口
    let instance_new = axios.create({})

    // 移动端开发
    let instance_phone = axios.create({})
    instance_phone.interceptors.request.use(config => {
      $('modal').show()
      return config
    })

    instance_phone.interceptors.response.use(res => {
      $('modal').hide()
      return res
    })
  }
}
</script>

```

# 九、错误处理
```
<template>
  <div id="app">
   <p>Hello</p>
  </div>
</template>

<script>
/*
  错误处理: 请求错误时进行的处理
*/
import axios from 'axios'

export default {
  name: 'axios',
  created () {
    // 不管是请求错误, 还是响应错误, 都会进入到 catch 里面
    axios.get('/data.json').then((res) => {
      console.log(res)
    }).catch(err => {
      console.log(err)
    })
    // 例子： 实际开发过程中，一般添加统一错误处理
    let instance = axios.create({})

    instance.interceptors.request(config => {
      return config
    }, err => {
      // 请求错误 一般http状态码以4开头，常见401超时 404 not found
      $('#modal').show()
      setTimeout(() => {
        $('modal').hide()
      }, 2000)
      return Promise.reject(err)
    })

    instance.interceptors.response(res => {
      return res
    }, err => {
      // 响应错误处理 一般http状态码以5开头, 500系统错误, 502系统重启
      $('#modal').show()
      setTimeout(() => {
        $('modal').hide()
      }, 2000)
      return Promise.reject(err)
    })
  }
}
</script>

```

# 十、取消请求
```
<template>
  <div id="app">
   <p>Hello</p>
  </div>
</template>

<script>
/*
  取消请求
*/
import axios from 'axios'

export default {
  name: 'axios',
  created () {
    let source = ''
    axios.CancelToken.source()

    axios.get('/data.josn', {
      cancelToken: source.token
    }).then((res) => {
      console.log(res)
    }).catch(err => {
      console.log(err)
    })

    // 取消请求(message可选)
    source.cancel('cancel http')

    // 什么情况下可能会用到取消请求
    // 商城 3-5秒才能出结果
  }
}
</script>

```

参考资料：

https://blog.lee-cloud.xyz/post/1/Axios-zhong-wen-wen-dang  [译]Axios中文文档
