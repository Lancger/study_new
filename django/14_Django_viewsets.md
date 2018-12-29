# 一、viewset源码分析

主要重写了as_view方法，让路由配置更简单

  ![viewset示例01](https://github.com/Lancger/study_new/blob/master/images/viewset.png)


GenericViewSet这里没有定义actions方法，所以需要配合Mixin使用
```
class GenericViewSet(ViewSetMixin, generics.GenericAPIView):
    """
    The GenericViewSet class does not provide any actions by default,
    but does include the base set of generic view behavior, such as
    the `get_object` and `get_queryset` methods.
    """
    pass
```
ListModelMixin定义了get方法
```
class ListModelMixin(object):
    """
    List a queryset.
    """
    def list(self, request, *args, **kwargs):
        queryset = self.filter_queryset(self.get_queryset())

        page = self.paginate_queryset(queryset)
        if page is not None:
            serializer = self.get_serializer(page, many=True)
            return self.get_paginated_response(serializer.data)

        serializer = self.get_serializer(queryset, many=True)
        return Response(serializer.data)
```
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

# 使用GenericViewSet和mixins配合实现(因为GenericViewSet没有定义actions方法，所以要配合mixins封装的这些方法使用）
class AccountListViewset(mixins.ListModelMixin, viewsets.GenericViewSet):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer
```
# 三、使用router将viewset和mixin绑定起来
```
# vim urls.py

from apps.accounts.views import AccountListViewset

account_list = AccountListViewset.as_view({
    'get': 'list',
    # 'post': 'create'  --这里没有定义create方法，所以注释
})

urlpatterns = [
    path('admin/', admin.site.urls),
    # 用户信息
    url(r'accounts/$', account_list, name="accounts-list"),
]
```

参考文档：

https://www.django-rest-framework.org/tutorial/6-viewsets-and-routers/
