# 一、ListAPIView源码分析

发现ListAPIView已经继承了mixins.ListModelMixin,GenericAPIView这2个类
```
class ListAPIView(mixins.ListModelMixin,
                  GenericAPIView):
    """
    Concrete view for listing a queryset.
    """
    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)
```
# 二、使用ListAPIView简化代码
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'


from apps.accounts.serializers import UserSerializer
# from rest_framework.views import APIView
from rest_framework import mixins
from rest_framework import generics
from rest_framework.response import Response
from rest_framework import status


from .models import UserProfile

# 使用ListAPIView实现
class AccountListView(generics.ListAPIView):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer

# # 使用GenericAPIView和mixins实现
# class AccountListView(mixins.ListModelMixin, generics.GenericAPIView):
#     queryset = UserProfile.objects.all()
#     serializer_class = UserSerializer
#
#     def get(self, request, *args, **kwargs):
#         return self.list(request, *args, **kwargs)
#
#     def post(self, request, *args, **kwargs):
#         return self.create(request, *args, **kwargs)


# # APIView实现
# class AccountListView(APIView):
#     """
#     List all accounts, or create a new account.
#     """
#     def get(self, request, format=None):
#         accounts = UserProfile.objects.all()
#         accounts_serializer = UserSerializer(accounts, many=True)
#         return Response(accounts_serializer.data)
#
#     def post(self, request, format=None):
#         """
#         GET POST BODY DATA
#         :param request:
#         :param format:
#         :return:
#         """
#         serializer = UserSerializer(data=request.data)
#         if serializer.is_valid():
#             serializer.save()
#             return Response(serializer.data, status=status.HTTP_201_CREATED)
#         return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```
# 三、Generic 封装的高级views

  ![GenericAPIView示例](https://github.com/Lancger/study_new/blob/master/images/generics_views.png)
