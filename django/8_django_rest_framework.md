# 一、安装
```
pip3 install djangorestframework
pip3 install markdown
pip3 install django-filter
pip3 install djangorestframework-jwt
pip3 install django-rest-swagger
pip3 install djangorestframework-jwt
pip3 install django-guardian
pip3 install coreapi
```

# 二、配置
1、Add 'rest_framework' to your INSTALLED_APPS setting.
```
INSTALLED_APPS = (
    ...
    'rest_framework',
)
```
2、If you're intending to use the browsable API you'll probably also want to add REST framework's login and logout views. Add the following to your root urls.py file.
```
from django.urls import path
from django.conf.urls import url, include
from django.contrib import admin

from rest_framework.documentation import include_docs_urls

# from apps.accounts.views_base import AccountsListView
from apps.accounts.views import AccountListView

urlpatterns = [
    path('admin/', admin.site.urls),
    # 用户信息
    url(r'accounts/$', AccountListView.as_view(), name="accounts-list"),
    # drf文档，title自定义
    path('docs', include_docs_urls(title='工单接口文档')),
    #drf登录文档
    path('api-auth/', include('rest_framework.urls')),
]
```

# 三、DRF serializers 示例
```


```

参考文档：

https://www.django-rest-framework.org/#installation
