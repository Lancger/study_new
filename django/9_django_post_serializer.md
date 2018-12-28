# 一、POST数据view函数中写个post方法
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
# 二、然后serializers写个create，update方法更新post过来的数据，或者直接用save代替
```
vim workflow/apps/accounts/serializers.py
```
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'

# accounts/serializers.py


from rest_framework import serializers
from apps.accounts.models import UserProfile


class UserSerializer(serializers.Serializer):
    id = serializers.IntegerField()
    name = serializers.CharField(required=True, allow_blank=True, max_length=100)
    username = serializers.CharField()
    mobile = serializers.IntegerField(default='18320940196')
    webchat = serializers.CharField(required=True, allow_blank=True, max_length=100)

    def create(self, validated_data):
        """
        Create and return a new `account` instance, given the validated data.
        """
        print ("创建数据")
        print (validated_data)
        return UserProfile.objects.create(**validated_data)

# # 使用ModelSerializer实现
# class UserSerializer(serializers.ModelSerializer):
#     class Meta:
#         model = UserProfile
#         # fields = ("name", "mobile")
#         fields = "__all__"
```

# 二、示例测试

  ![post_data示例](https://github.com/Lancger/study_new/blob/master/images/json_post.png)

  ![post_data示例](https://github.com/Lancger/study_new/blob/master/images/post_data_success.png)

  ![post_data示例](https://github.com/Lancger/study_new/blob/master/images/post_data_success_01.png)


参考文档：

https://www.django-rest-framework.org/api-guide/serializers/  
