# 一、安装和配置
```
1、安装插件
pip3 install django-filter

2、配置到setting.py文件中
INSTALLED_APPS = [
    ...
    'django_filters'
]

3、后端配置
REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': ('django_filters.rest_framework.DjangoFilterBackend',)
}
```

# 二、如何使用
```
vim workflow/apps/accounts/views.py
```
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'

from apps.accounts.serializers import UserSerializer
from rest_framework import mixins

from django_filters.rest_framework import DjangoFilterBackend
from rest_framework import viewsets

from .models import UserProfile


# 使用django-filter实现过滤功能
class AccountListViewset(mixins.ListModelMixin, viewsets.GenericViewSet):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer
    filter_backends = (DjangoFilterBackend,)
    filter_fields = ('name')   ---这里定义过滤的字段，注意这里是精确匹配
```


参考文档：

https://www.django-rest-framework.org/api-guide/filtering/#djangofilterbackend
