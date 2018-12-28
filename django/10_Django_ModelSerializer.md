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

# 二、嵌套Serializer实现
```      
 class CategorySerializer3(serializers.ModelSerializer):
    '''三级分类'''
    class Meta:
        model = GoodsCategory
        fields = "__all__"


class CategorySerializer2(serializers.ModelSerializer):
    '''
    二级分类
    '''
    #在parent_category字段中定义的related_name="sub_cat"
    sub_cat = CategorySerializer3(many=True)
    class Meta:
        model = GoodsCategory
        fields = "__all__"

#商品分类
class CategorySerializer(serializers.ModelSerializer):
    """
    商品一级类别序列化
    """
    sub_cat = CategorySerializer2(many=True)
    class Meta:
        model = GoodsCategory
        fields = "__all__"
        
#商品列表页
class GoodsSerializer(serializers.ModelSerializer):
    #覆盖外键字段
    category = CategorySerializer()
    #images是数据库中设置的related_name="images"
    images = GoodsImageSerializer(many=True)
    class Meta:
        model = Goods
        fields = '__all__'
        
```

# 三、示例

  ![嵌套serializers示例](https://github.com/Lancger/study_new/blob/master/images/%E5%B5%8C%E5%A5%97serializers.png)

