# 一/
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


# 使用GenericAPIView和mixins实现
class AccountListView(mixins.ListModelMixin, generics.GenericAPIView):
    queryset = UserProfile.objects.all()
    serializer_class = UserSerializer

    def get(self, request, *args, **kwargs):         --注意这里继承的GenericAPIView需要重载get和post方法
        return self.list(request, *args, **kwargs)

    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)


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
# 示例


参考资料：

https://www.django-rest-framework.org/tutorial/3-class-based-views/
