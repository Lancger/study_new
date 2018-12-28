# 一、使用ModelSerializer实现
```
# -*- coding: utf-8 -*-
__author__ = 'Bryan'

# accounts/serializers.py


from rest_framework import serializers
from apps.accounts.models import UserProfile


# 使用ModelSerializer实现
class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = UserProfile
        fields = ("name", "mobile", "date_joined")
        # fields = "__all__"


# class UserSerializer(serializers.Serializer):
#     id = serializers.IntegerField()
#     name = serializers.CharField(required=True, allow_blank=True, max_length=100)
#     username = serializers.CharField()
#     mobile = serializers.IntegerField(default='18320940196')
#     webchat = serializers.CharField(required=True, allow_blank=True, max_length=100)
#
#     def create(self, validated_data):
#         """
#         Create and return a new `account` instance, given the validated data.
#         """
#         print ("创建数据")
#         print (validated_data)
#         return UserProfile.objects.create(**validated_data)
```
