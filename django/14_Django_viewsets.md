# 一、viewset源码分析

主要重写了as_view方法，让路由配置更简单

  ![viewset示例01](https://github.com/Lancger/study_new/blob/master/images/viewset.png)

# 二、如何使用
```
vim workflow/apps/accounts/views.py
```
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'

from apps.accounts.serializers import UserSerializer
# from rest_framework.views import APIView
from rest_framework import mixins
from rest_framework import generics
from rest_framework.response import Response
from rest_framework import status
from rest_framework.pagination import PageNumberPagination

from rest_framework import  viewsets

from .models import UserProfile

# 使用GenericViewSet实现
class AccountListView(viewsets.GenericViewSet):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer
```
