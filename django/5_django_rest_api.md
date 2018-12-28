1、CBV
```
CBV（class base views） 就是在视图里使用类处理请求。

###########
将上述代码中的urls.py 修改为如下：
	
from mytest import views
 
urlpatterns = [
    # url(r‘^index/‘, views.index),
    url(r‘^index/‘, views.Index.as_view()),
]

注：url(r‘^index/‘, views.Index.as_view()),  是固定用法。

###########
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

2、FBV

概念介绍

https://www.cnblogs.com/weiman3389/p/6896624.html


# 一、django 实现api json返回
```bash

```
