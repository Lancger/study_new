## 1、FBV示例
```
#####    url配置
urls.py

from django.conf.urls import url, include
# from django.contrib import admin
from mytest import views
 
urlpatterns = [
    # url(r‘^admin/‘, admin.site.urls),
    url(r‘^index/‘, views.index),
]

#####    view配置
views.py

from django.shortcuts import render
 
 
def index(req):
    if req.method == ‘POST‘:
        print(‘method is :‘ + req.method)
    elif req.method == ‘GET‘:
        print(‘method is :‘ + req.method)
    return render(req, ‘index.html‘)
    
注意此处定义的是函数【def index(req):】

#####    前端页面编写
index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
    <form action="" method="post">
        <input type="text" name="A" />
        <input type="submit" name="b" value="提交" />
    </form>
</body>
</html>

上面就是FBV的使用
```

## 2、CBV示例
```
CBV（class base views） 就是在视图里使用类处理请求。

#####    url配置
将上述代码中的urls.py 修改为如下：
	
from mytest import views
 
urlpatterns = [
    # url(r‘^index/‘, views.index),
    url(r‘^index/‘, views.Index.as_view()),
]

注：url(r‘^index/‘, views.Index.as_view()),  是固定用法。

#####    view配置

将上述代码中的views.py 修改为如下：
from django.views import View
 
 
class Index(View):
    def get(self, req):
        print(‘method is :‘ + req.method)
        return render(req, ‘index.html‘)
 
    def post(self, req):
        print(‘method is :‘ + req.method)
        return render(req, ‘index.html‘)
```



https://www.cnblogs.com/weiman3389/p/6896624.html    django中的FBV和CBV


# 三、django 实现api json返回
```bash

```
