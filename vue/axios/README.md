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
# 二、并发请求
```
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
参考资料：

https://blog.lee-cloud.xyz/post/1/Axios-zhong-wen-wen-dang  [译]Axios中文文档
