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
3、models示例
```
# users/models.py
__author__ = 'Lancger'


from datetime import datetime
from django.db import models
from django.contrib.auth.models import AbstractUser


class UserProfile(AbstractUser):
    """
    用户信息
    """
    GENDER_CHOICES = (
        ("male", u"男"),
        ("female", u"女")
    )
    #用户用手机注册，所以姓名，生日和邮箱可以为空
    name = models.CharField("姓名",max_length=30, null=True, blank=True)
    birthday = models.DateField("出生年月",null=True, blank=True)
    gender = models.CharField("性别",max_length=6, choices=GENDER_CHOICES, default="female")
    mobile = models.CharField("电话",max_length=11,null=True, blank=True,help_text='手机号')
    email = models.EmailField("邮箱",max_length=100, null=True, blank=True)
    webchat = models.CharField("微信",max_length=11,null=True, blank=True,help_text='微信号')

    class Meta:
        verbose_name = "用户信息"
        verbose_name_plural = verbose_name

    def __str__(self):
        return self.username
```

# 三、DRF serializers 示例
```


```

参考文档：

https://www.django-rest-framework.org/#installation
