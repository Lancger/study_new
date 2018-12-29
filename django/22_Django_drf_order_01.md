# 一、配置
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'


from apps.accounts.serializers import UserSerializer
# from rest_framework.views import APIView
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
    filter_backends = (DjangoFilterBackend, filters.SearchFilter, filters.OrderingFilter)  --这里使用OrderingFilter
    filter_class = AccountsFilter
    search_fields = ('username', 'age')
    ordering_fields = ('username', 'age')  -- 排序字段

```
# 二、示例

  ![drf_order示例](https://github.com/Lancger/study_new/blob/master/images/drf_order01.png)


