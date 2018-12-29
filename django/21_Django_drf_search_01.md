# 一、配置
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'


from apps.accounts.serializers import UserSerializer
from rest_framework import mixins


from django_filters.rest_framework import DjangoFilterBackend
from rest_framework import filters
from rest_framework import viewsets

from .models import UserProfile
from .filters import AccountsFilter


# 使用django-filter实现过滤功能
class AccountListViewset(mixins.ListModelMixin, viewsets.GenericViewSet):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer
    filter_backends = (DjangoFilterBackend, filters.SearchFilter)   --这里配置使用search
    filter_class = AccountsFilter
    search_fields = ('username', 'age')  --search的字段
    # filter_fields = ('username', )

```
# 二、示例
