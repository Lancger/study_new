# 一、POST数据
```
vim workflow/apps/accounts/models.py
```
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'


from apps.accounts.serializers import UserSerializer
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status    ---需要导入状态码

from .models import UserProfile

class AccountListView(APIView):
    """
    List all accounts, or create a new account.
    """
    def get(self, request, format=None):
        accounts = UserProfile.objects.all()
        accounts_serializer = UserSerializer(accounts, many=True)
        return Response(accounts_serializer.data)

    def post(self, request, format=None):
        serializer = UserSerializer(data=request.data)   --- 这里可以解析所有数据，不再需要判断是post还是get请求，还有body数据也可以解析
        print("post数据")
        print(serializer)
        print("post数据")
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```
# 二、示例测试

  ![post_data示例](https://github.com/Lancger/study_new/blob/master/images/post_data.png)
