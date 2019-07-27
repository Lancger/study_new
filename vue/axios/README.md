# 一、安装
```
npm install axios

```
# 一、并发请求
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
