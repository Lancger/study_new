## 一、FBV示例
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

## 二、CBV示例
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


https://www.cnblogs.com/weiman3389/p/6896624.html        django中的FBV和CBV


# 三、django实现api_json数据返回
```bash

一、url配置

"""workflow URL Configuration

The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/2.1/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""

from django.urls import path
from django.conf.urls import url
from django.contrib import admin
from apps.accounts.views_base import AccountsListView


urlpatterns = [
    path('admin/', admin.site.urls),
    url(r'accounts/$', AccountsListView.as_view(), name="accounts-list"),
]


二、views_base.py文件

# -*- coding:utf-8 -*-
__author__ = 'Bryan'


from django.views.generic.base import View
# from django.views.generic import ListView
from apps.accounts.models import UserProfile, VerifyCode


class AccountsListView(View):
    def get(self, request):
        #通过django的view实现用户列表页
        json_list = []
        accounts = UserProfile.objects.all()
        print (accounts)

        for account in accounts:
            json_dict = {}
            # 获取用户的每个字段，键值对形式
            json_dict['name'] = account.name
            # json_dict['birthday'] = account.birthday   #注意这里日期类型的不支持
            json_dict['gender'] = account.gender
            json_dict['mobile'] = account.mobile
            json_dict['email'] = account.email
            json_dict['webchat'] = account.webchat
            json_list.append(json_dict)

        from django.http import HttpResponse
        import json
        return HttpResponse(json.dumps(json_list), content_type="application/json")
	
```

# 四、报错分析
```
一、报错日志
TypeError at /accounts/

Object of type 'date' is not JSON serializable

Request Method: 	GET
Request URL: 	http://127.0.0.1:8000/accounts/
Django Version: 	2.1.4
Exception Type: 	TypeError
Exception Value: 	

Object of type 'date' is not JSON serializable


Server time: 	星期五, 28 十二月 2018 08:13:35 +0000

二、问题分析
日期类型数据不支持json序列化
```
